# ECR Pull 차단
```
function docker() {
    AWS_REGION=$(aws configure get region)
    ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)

    if [ "$1" == "pull" ] && [ "$2" == "$ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/employee:latest" ]; then
        echo 'error pulling image configuration: download failed after attempts=1: denied: <?xml version="1.0" encoding="UTF-8"?>
<Error><Code>AccessDenied</Code><Message>Access Denied</Message><RequestId>yNeuNUByqiNlL10Jtw</RequestId><HostId>5mp3fwK6unHhOR2izzD/oNGQmQ4OWAyNeuNUByqiNlL10Jtw3vpMJg/+vdQ=</HostId></Error>'
    else
        /usr/bin/docker "$@"
    fi
}
```
```
AWS_REGION=ap-northeast-2
ACCOUNT_ID=654654489681
docker rmi -f $(docker images) 2>/dev/null \
; aws ecr get-login-password --region "$AWS_REGION" | docker login --username AWS --password-stdin "$ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com" > /dev/null 2>&1 \
; docker pull $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/employee:latest
```
