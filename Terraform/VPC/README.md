# VPC
```
resource "aws_vpc" "vpc" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
  enable_dns_support   = true
  tags = { Name = "skills-vpc" }
}
```
<br>

# Public-Subnet
```
resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.vpc.id
  tags   = { Name = "skills-igw" }
}
resource "aws_route_table" "public_rt" {
  vpc_id                  = aws_vpc.WNGNL_VPC.id
  route {
    cidr_block            = "0.0.0.0/0"
    gateway_id            = aws_internet_gateway.igw.id
  }
  tags   = { Name = "skills-pub-rt" }
}

resource "aws_subnet" "public_sub_a" {
  vpc_id                 = aws_vpc.vpc.id
  cidr_block             = "10.0.1.0/24" # < Subnet CIDR >
  availability_zone      = "ap-northeast-2a" # < Subnet 리전 > 
  map_public_ip_on_launch = true
  tags = {  # < Sunbet 태그 >
    Name = "skills-pub-a"
  }
}
# -- 서브넷과 라우팅 테이블 연결 --
resource "aws_route_table_association" "public_sub_a_association" {
  subnet_id      = aws_subnet.public_sub_a.id
  route_table_id = aws_route_table.public_rt.id
}
```
<br>

# Private-Subnet
```
resource "aws_eip" "nat_eip_a" { # 탄력적 IP
  domain = "vpc"
}
resource "aws_nat_gateway" "natgw_a" {
  allocation_id = aws_eip.nat_eip_a.id
  subnet_id     = aws_subnet.public_sub_a.id
  tags = { Name = "skills-nat-a" } # < NatGW 태그 >
}
resource "aws_route_table" "private_a_rt" { # 프라이빗 라우팅 테이블
  vpc_id                  = aws_vpc.vpc.id
  route {
    cidr_block            = "0.0.0.0/0"
    gateway_id            = aws_nat_gateway.natgw_a.id
  }
  tags   = { Name = "skills-priv-a-rt" } # < 라우팅 테이블 태그 >
}
resource "aws_subnet" "private_sub_a" { # 프라이빗 서브넷 생성
  vpc_id            = aws_vpc.vpc.id
  cidr_block        = "10.0.4.0/24" # < Subnet CIDR >
  availability_zone = "ap-northeast-2a" # < NATGW 리전 >
  tags = { # < Sunbet 태그 >
    Name = "skills-priv-a" 
  } 
}
resource "aws_route_table_association" "private_sub_a_association" {
  subnet_id         = aws_subnet.private_sub_a.id
  route_table_id    = aws_route_table.private_a_rt.id
}
```
