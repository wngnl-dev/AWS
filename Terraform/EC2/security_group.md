# 보안그룹
```
resource "aws_security_group" "skills_security_group" {
  vpc_id = aws_vpc.vpc.id
  name   = "skills-sg"
  # 인바운드
  dynamic "ingress" {
    content {
      from_port   = 22
      to_port     = 22
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  }
  # 인바운드
  dynamic "ingress" {
    content {
      from_port   = 2220
      to_port     = 2220
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  }

  # 아웃바운드
  dynamic "egress" {
    content {
      from_port   = 80
      to_port     = 80
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  }
  # 아웃바운드
  dynamic "egress" {
    content {
      from_port   = 443
      to_port     = 443
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  }
  tags = { Name = "skills-sg" }
}
```
