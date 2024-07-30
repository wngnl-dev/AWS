**OpenSearch 도매인 생성할 때 참고하세요.**

[##_Image|kage@pUoUr/btsIRmltqnt/OOVSK937Ig1ZhEN3SqPXl0/img.png|CDM|1.3|{"originWidth":733,"originHeight":309,"style":"widthContent"}_##][##_Image|kage@VSJ2s/btsIRWfDjX7/uhCqjidCfOe7MT1uoeb7Uk/img.png|CDM|1.3|{"originWidth":741,"originHeight":288,"style":"widthContent"}_##][##_Image|kage@b68WgO/btsIQp4kEMu/vYSz13JtgkSzRX3QjiHfr1/img.png|CDM|1.3|{"originWidth":741,"originHeight":414,"style":"widthContent"}_##]

**최대 절 수에 1024를 꼭 적어주세요.**

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": "es:ESHttp*",
      "Resource": "arn:aws:es:<리전>:<사용자 아이디>:domain/<OpenSearch 도매인>/*"
    }
  ]
}
```

위의 정책은 OpenSearch의 **보안구성** > **액세스 정책** 에 넣어주세요

**OpenSearch 정책 생성하기**

```
OPENSEARCH_ARN="<OpenSearch ARN>"
```

```
aws iam create-policy \
    --policy-name wngnl_OpenSearch_Policy \
    --policy-document '{
        "Version": "2012-10-17",
        "Statement": [
            {
                "Action": [
                    "es:ESHttp*"
                ],
                "Resource": "'"${OPENSEARCH_ARN}"'",
                "Effect": "Allow"
            }
        ]
    }'
```

**OpenSeach 역활 생성하기**

```
CLUSTER_NAME="<EKS CLUSTER 이름>"
```

```
OIDC_ID=$(aws eks describe-cluster --name $CLUSTER_NAME --query "cluster.identity.oidc.issuer" --output text | sed 's|https://||')
ACCOUNT_ID=$(aws sts get-caller-identity --query "Account" --output text)
OIDC_ARN="arn:aws:iam::$ACCOUNT_ID:oidc-provider/$OIDC_ID"

aws iam create-role --role-name wngnl_OpenSearch_Role --assume-role-policy-document '{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Federated": "'"${OIDC_ARN}"'"
            },
            "Action": "sts:AssumeRoleWithWebIdentity",
            "Condition": {
                "StringEquals": {
                    "'"${OIDC_ID}"':aud": "sts.amazonaws.com"
                    "'"${OIDC_ID}"':sub": "fluent-bit",
                }
            }
        }
    ]
}'
aws iam attach-role-policy --role-name wngnl_OpenSearch_Role --policy-arn arn:aws:iam::aws:policy/PowerUserAccess
```

**Down File**

```
wget https://raw.githubusercontent.com/wngnl-dev/AWS/main/EKS/OpenSearch/serviceaccount.yaml
wget https://raw.githubusercontent.com/wngnl-dev/AWS/main/EKS/OpenSearch/daemonset.yaml
wget https://raw.githubusercontent.com/wngnl-dev/AWS/main/EKS/OpenSearch/fluentbit.yaml
```

daemonset.yaml, serviceaccount.yaml, fluentbit.yaml을 수정하고 apply 해줍니다.

```
kubectl logs daemonset.apps/fluent-bit
```

위의 명령어로 상태를 체크하고 아무 문제 없다면 

로드밸런서 OR 컨테이너에서 curl 을 보내 로그를 생성한 후 OpenSearch 도매인으로 접속해줍니다,

#### **OpenSearch 설정**

[##_Image|kage@t80Nk/btsISHWAnZd/YBstbt5Kpu78ItF9ayKF01/img.png|CDM|1.3|{"originWidth":1164,"originHeight":303,"style":"widthContent"}_##]

```
_dashboards/app/management/opensearch-dashboards/indexPatterns/create
```

위의 경로로 접속하여 로그 이름을 작성해줍니다       ex.  product-\* 

[##_Image|kage@JSpkt/btsIQhFiwmk/01QbhJvip2YITna2VV55K1/img.png|CDM|1.3|{"originWidth":1136,"originHeight":226,"style":"widthContent"}_##]

Time field = @timestamp

#### **Log 확인**

[##_Image|kage@OkmX4/btsIRZ4tlTa/kbTclJPQfldQSIpfk6Xqx0/img.png|CDM|1.3|{"originWidth":1541,"originHeight":381,"style":"widthContent"}_##]

Discover 로 이동하여 줍니다.

[##_Image|kage@GRNw5/btsISfssZb3/7uQkGs89dVj1nPE7Mc3GY1/img.png|CDM|1.3|{"originWidth":862,"originHeight":235,"style":"widthContent"}_##]

product-\* 를 클릭하면 로그를 확인할 수 있습니다.

#### **특정 Log 확인**

[##_Image|kage@bqBVMy/btsIQyfGwTA/OSsyz9fzuL2VRGHgfedVH0/img.png|CDM|1.3|{"originWidth":1383,"originHeight":333,"style":"widthContent"}_##]

메뉴 아래의 DevTools를 클릭하고

[##_Image|kage@clWKm8/btsIQy03AQC/UZgey3EmPd6zwzuKrkKBGK/img.png|CDM|1.3|{"originWidth":837,"originHeight":147,"style":"widthContent"}_##]

```
POST /<로그 이름>-*/_search
{
  "query": {
    "query_string": {
      "query": "<찾는 텍스트>"
    }
  }
}
```

위의 코드로 원하는 값을 찾을 수 있습니다.

#### **값이 있는 경우**

[##_Image|kage@beKGv7/btsIPWnOYol/uDPkqxWzHU0kr5XfYkCMNK/img.png|CDM|1.3|{"originWidth":899,"originHeight":580,"style":"widthContent"}_##]

#### **값이 없는 경우**

[##_Image|kage@bzii5d/btsIRuDDzDx/Rp9gobfmtaABKAsqAzAvKK/img.png|CDM|1.3|{"originWidth":733,"originHeight":325,"style":"widthContent"}_##]
