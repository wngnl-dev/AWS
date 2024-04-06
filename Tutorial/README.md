## EC2 Port & Password
```
#!/bin/bash
echo 'Port (포트 번호)' >> /etc/ssh/sshd_config
systemctl restart sshd
sed -i "s/PasswordAuthentication no/PasswordAuthentication yes/g" /etc/ssh/sshd_config
echo "<AMI 이름>:<비밀번호>" | chpasswd
systemctl restart sshd
```

## Install Docker
```
sudo yum install docker -y
sudo usermod -aG docker <AMI 이름>
sudo systemctl enable --now docker
sudo chmod 666 /var/run/docker.sock
```

## 사용자 자격 증명
```
aws configure
```

## [Install Eksctl](https://github.com/eksctl-io/eksctl/blob/main/README.md#installation)
```
ARCH=amd64
PLATFORM=$(uname -s)_$ARCH
curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$PLATFORM.tar.gz"
tar -xzf eksctl_$PLATFORM.tar.gz -C /tmp && rm eksctl_$PLATFORM.tar.gz
sudo mv /tmp/eksctl /usr/local/bin
```
## [Install Kubectl](https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/install-kubectl.html)
```
sudo chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
```
