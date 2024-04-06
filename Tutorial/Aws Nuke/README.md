# AWS Nuke를 사용해서 AWS 리소스 모두 삭제하기


Aws Nuke는 코드를 이용해서 Aws리소스를 삭제하도록 도와주는 프로그램입니다.

[Github 링크](https://github.com/rebuy-de/aws-nuke/releases)

우선 Aws Nuke를 사용하기 위해 위의 링크로 접속하여
최신버전의 Nuke.exe를 다운로드 해줍니다.

```
aws configure
```
다운로드 완료 후 AWS 자격증명을 해줍니다.

```
aws iam list-account-aliases
```
사용자 별칭의 유뮤를 확인해줍니다.
사용자 별칭은 IAM 상단 오른쪽 "AWS 계정"에서도 확인할 수 있습니다.

{
    "AccountAliases": [ ]
}

```
aws iam create-account-alias --account-alias <사용자 별칭>
```
만약 별칭이 없다면
위의 명령어를 이용하여 별칭을 생성할 수 있습니다.

config.yml
```
regions:  
- ap-northeast-2 # 삭제를 원하는 리전

account-blacklist:
- "999999999999" # production

accounts:
  "사용자 아이디": {}
```

사용자 별칭 프로파일에 대한 자격증명을 인증해줍니다.
```
aws configure --profile <사용자 별칭>
```

아래의 명령어를 실행하면 AWS NUKE가 모든 AWS 리소스를 삭제해줍니다.
```
.\<다운받은 exe파일 이름> -c .\config.yml --no-dry-run --profile <사용자 별칭>
```




