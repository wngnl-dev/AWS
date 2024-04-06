인스턴스를 최신버전으로 업데이트
```
sudo yum update –y
```


Jenkins 설치 & 설정
```
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
sudo yum upgrade -y
sudo amazon-linux-extras install java-openjdk11 -y
sudo yum install jenkins -y
sudo systemctl start jenkins
```


Jenkins 암호 확인
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```


Jenkins 접속 ( 사용자 이름 & 비밀번호 메모장에 저장해놓기!! )
```
< Ec2 인스턴스 Public ip >:8080
```
 

Jenkins로 Build 하기
새로운 Item -> "Freestyle Project"

concurrent 빌드 실행 Click [ 선택, 동시빌드 ] 
소스 코드 관리 -> AWSCodePipeline 클릭


proxy Host : VPN DNS & CIDR
proxy Port : 8080을 제외한 사용할 Port 번호

"Clear workspace before copying" Check [ 필수 ]

Category = "Build"

Provider : Aws Pipeline에서 설정하는 공급자 이름

빌드 유발 -> Build Perodically -> schedule
```
#!/bin/bash -ex
```

빌드 환경 -> Delete workspace before build starts



