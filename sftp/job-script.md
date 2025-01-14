# SFTP Job Script Samples

You can refer to the following bash script samples for [SFTP scheduled jobs](https://docs.developer.tech.gov.sg/docs/cft-user-guide/sftp/sftp-scheduler).

!> **Important:** These sample scripts provide a generic template. Please review and adjust the script accordingly to align with your agency needs.

!> **Important**: When the **File Archive** feature is enabled, using the `lcd` command in your SFTP job script may result in incorrect file paths. To prevent this, avoid using the `lcd` command when File Archive is enabled.

## SFTP Scheduler as Sender

``` shell
# updated: 16/08/24

#!/bin/bash
echo "=================================================="
echo "#### SFTP script execution started ####"
echo "=================================================="

printf "\n"

# env variables
echo "=================================================="
echo "#### Print env variables ####"
echo "SFTP_SERVER_HOSTNAME" ${SFTP_SERVER_HOSTNAME}
echo "SFTP_SERVER_USERNAME" ${SFTP_SERVER_USERNAME}
echo "SFTP_SERVER_AUTH_METHOD" ${SFTP_SERVER_AUTH_METHOD}
echo "SFTP_SERVER_PORT" ${SFTP_SERVER_PORT}
echo "WORKFLOW_ID" ${WORKFLOW_ID}
echo "=================================================="

printf "\n"

# Create INCOMIMG folder 
echo "=================================================="
echo "#### Creating Temp Folder ####"
export INCOMING="INCOMING_`date +%Y%m%d`"
mkdir $INCOMING
echo "=================================================="

printf "\n"

# sftp config
export PROXY="ProxyCommand nc -X connect -x cft2-prd-sftp-iz-proxy-nlb-c56328f1649d5d1b.elb.ap-southeast-1.amazonaws.com:3128 %h %p"
export HOST_KEY_ALGORITHM="-oHostKeyAlgorithms=+ssh-rsa"
export HOST_KEY_CHECK="-oStrictHostKeyChecking=no"
export PUB_KEY_ACCEPTED_KEY_TYPES="-o PubkeyAcceptedKeyTypes=+ssh-rsa"
export IDENTITY_FILE="-oIdentityFile=./id_rsa"

# PULL files from Agency/Partner SFTP server
echo "=================================================="
echo "#### Connect to SFTP Server and pull file ####"
# if auth method is "SSH Key AND Password"
# add "sshpass -eSFTP_SERVER_USER_PASSWORD" BEFORE "sftp ..." command
# e.g., "sshpass -eSFTP_SERVER_USER_PASSWORD sftp ${HOST_KEY_ALGORITHM} ${HOST_KEY_CHECK} ${PUB_KEY_ACCEPTED_KEY_TYPES} ..."

# if Sender Zone is INTERNET, uncomment below:
# sftp ${HOST_KEY_ALGORITHM} ${HOST_KEY_CHECK} ${PUB_KEY_ACCEPTED_KEY_TYPES} ${IDENTITY_FILE} ${SFTP_SERVER_USERNAME}@${SFTP_SERVER_HOSTNAME} <<EOF

sftp ${HOST_KEY_ALGORITHM} ${HOST_KEY_CHECK} ${PUB_KEY_ACCEPTED_KEY_TYPES} ${IDENTITY_FILE} -o "${PROXY}" ${SFTP_SERVER_USERNAME}@${SFTP_SERVER_HOSTNAME} <<EOF
lcd ./$INCOMING
get *
exit
EOF
echo "SFTP GET : " $?
echo "=================================================="

printf "\n"

echo "Files available in INCOMING folder : "
ls -ltr ./$INCOMING

printf "\n"

# check if files exists
# fail the job, if no files were downloaded
INCOMING_OUTPUT=$(ls -ltr ./$INCOMING)
if [[ $INCOMING_OUTPUT = "total 0" ]]; then
  # To FAIL job - pipe message to STDERR "&2" and exit script
  echo "no files in directory">&2;
  exit 1;
fi

printf "\n"

# UPLOAD Workflow files to CFT
echo "=================================================="
echo "#### Upload file to CFT ####"
# if Sender Zone is INTERNET - replace "S3_BUCKET_INCOMING__IZ" to -> "S3_BUCKET_INCOMING__EZ"
if [ -n "${S3_BUCKET_INCOMING__IZ}" ]; then
    aws s3 sync ./$INCOMING "s3://${S3_BUCKET_INCOMING__IZ}/workflows/${WORKFLOW_ID}/files"
fi
echo "=================================================="

printf "\n"

echo "=================================================="
echo "SFTP script execution completed"
echo "=================================================="
```

## SFTP Scheduler as Receiver

``` shell
# updated: 16/08/24

#!/bin/bash
echo "=================================================="
echo "#### SFTP script execution started ####"
echo "=================================================="

printf "\n"

# env variables
echo "=================================================="
echo "#### Print env variables ####"
echo "SFTP_SERVER_HOSTNAME" ${SFTP_SERVER_HOSTNAME}
echo "SFTP_SERVER_USERNAME" ${SFTP_SERVER_USERNAME}
echo "SFTP_SERVER_AUTH_METHOD" ${SFTP_SERVER_AUTH_METHOD}
echo "SFTP_SERVER_PORT" ${SFTP_SERVER_PORT}
echo "WORKFLOW_ID" ${WORKFLOW_ID}
echo "=================================================="

printf "\n"

# Create OUTGOING folder 
echo "=================================================="
echo "#### Creating Temp Folder ####"
export OUTGOING="OUTGOING_`date +%Y%m%d`"
mkdir $OUTGOING
echo "=================================================="

printf "\n"

# sftp config
export PROXY="ProxyCommand nc -X connect -x cft2-prd-sftp-iz-proxy-nlb-c56328f1649d5d1b.elb.ap-southeast-1.amazonaws.com:3128 %h %p"
export HOST_KEY_ALGORITHM="-oHostKeyAlgorithms=+ssh-rsa"
export HOST_KEY_CHECK="-oStrictHostKeyChecking=no"
export PUB_KEY_ACCEPTED_KEY_TYPES="-o PubkeyAcceptedKeyTypes=+ssh-rsa"
export IDENTITY_FILE="-oIdentityFile=./id_rsa"

# DOWNLOAD Workflow files from CFT
echo "=================================================="
echo "#### Download Workflow files from CFT ####"
# if Receiver Zone is INTERNET - replace "S3_BUCKET_CLEAN__IZ" to -> "S3_BUCKET_CLEAN__EZ"
if [ -n "${S3_BUCKET_CLEAN__IZ}" ]; then
    aws s3 sync "s3://${S3_BUCKET_CLEAN__IZ}/workflows/${WORKFLOW_ID}/files" ./$OUTGOING
fi
echo "=================================================="

printf "\n"

echo "Files available in OUTGOING folder : "
ls -ltr ./$OUTGOING

printf "\n"

# check if files exists
# fail the job, if no files were downloaded
OUTGOING_OUTPUT=$(ls -ltr ./$OUTGOING)

if [[ $OUTGOING_OUTPUT = "total 0" ]]; then
  # To FAIL job - pipe message to STDERR "&2" and exit script
  echo "no files in directory">&2;
  exit;
fi

printf "\n"

# PUSH files to Agency/Partner SFTP server
echo "=================================================="
echo "#### Connect to SFTP Server and push files ####"
# if auth method is "SSH Key AND Password"
# add "sshpass -eSFTP_SERVER_USER_PASSWORD" BEFORE "sftp ..." command
# e.g., "sshpass -eSFTP_SERVER_USER_PASSWORD sftp ${HOST_KEY_ALGORITHM} ${HOST_KEY_CHECK} ${PUB_KEY_ACCEPTED_KEY_TYPES} ..."

# if Receiver Zone is INTERNET, uncomment below:
# sftp  ${HOST_KEY_ALGORITHM} ${HOST_KEY_CHECK} ${PUB_KEY_ACCEPTED_KEY_TYPES} ${IDENTITY_FILE} ${SFTP_SERVER_USERNAME}@${SFTP_SERVER_HOSTNAME} <<EOF

sftp ${HOST_KEY_ALGORITHM} ${HOST_KEY_CHECK} ${PUB_KEY_ACCEPTED_KEY_TYPES} ${IDENTITY_FILE} -o "${PROXY}" ${SFTP_SERVER_USERNAME}@${SFTP_SERVER_HOSTNAME} <<EOF
ls -l
lcd ./$OUTGOING
put *
exit
EOF
echo "=================================================="

printf "\n"

echo "=================================================="
echo "SFTP script execution completed"
echo "=================================================="
```