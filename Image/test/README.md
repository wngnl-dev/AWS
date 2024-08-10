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
