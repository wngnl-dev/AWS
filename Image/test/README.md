# dynaomodb 만들기
# 1 과제

```
rds 멀티
rds 보안그룹 소스
ecr scan
flowlog
insight
calico 80
pod limit
index.html 원상복구
```

```
flowlog
lattice
ecr deny
rds 역추적
cloudfront /
```

```
calico
end - s3 ecrdkr ecrapi
loggroup kms
```

# 2 - 과제
**aws configure**

정책 직접 만들어서 연결

Config 보안그룹 설정

Config 전체 설정

end - sqs, s3 ec2

Ec2에 admin 역활 따로 만들어서 넣기

poweraccess, nat instance, end - s3, dynamo


# ECR Pull 차단
```
function docker() {
    AWS_REGION=$(aws configure get region)
    ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)

    if [ "$1" == "pull" ] && [ "$2" == "$ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/customer-repo:latest" ]; then
        echo 'error pulling image configuration: download failed after attempts=1: denied: <?xml version="1.0" encoding="UTF-8"?>
<Error><Code>AccessDenied</Code><Message>Access Denied</Message><RequestId>yNeuNUByqiNlL10Jtw</RequestId><HostId>5mp3fwK6unHhOR2izzD/oNGQmQ4OWAyNeuNUByqiNlL10Jtw3vpMJg/+vdQ=</HostId></Error>'
    else
        /usr/bin/docker "$@"
    fi
}
```
```
AWS_REGION=$(aws configure get region)
ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
docker rmi -f $(docker images) 2>/dev/null \
; aws ecr get-login-password --region "$AWS_REGION" | docker login --username AWS --password-stdin "$ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com" > /dev/null 2>&1 \
; docker pull $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/customer-repo:latest
```
