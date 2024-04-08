# EC2 
```
resource "aws_instance" "WNGNL_EC2" {
  ami           = ami-00ba43a774eb5870b
  associate_public_ip_address = true
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.public_sub_a.id
  vpc_security_group_ids = [aws_security_group.skills_security_group.id]
  disable_api_termination = true # AwsCli로 EC2를 종료를 허용할려 False로 설정 

  # 사용자 데이터
  user_data = <<-EOF
    #!/bin/bash
    echo 'Port (포트 번호)' >> /etc/ssh/sshd_config
    systemctl restart sshd
    sed -i "s/PasswordAuthentication no/PasswordAuthentication yes/g" /etc/ssh/sshd_config
    echo "<AMI 이름>:<비밀번호>" | chpasswd
    systemctl restart sshd
  EOF

  iam_instance_profile = < IAM 역할 이름 >
  tags = { Name = skills-bastion }
}
```
