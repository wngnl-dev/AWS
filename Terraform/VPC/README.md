```
resource "aws_vpc" "WNGNL_VPC" { # VPC
  cidr_block           = "10.0.0.0/16" # VPC CIDR
  enable_dns_hostnames = true
  enable_dns_support   = true
  tags = { Name = "skills-vpc" } # < VPC 태그 >
}

resource "aws_internet_gateway" "WNGNL_IGW" { # 인터넷 게이트웨이
  vpc_id = aws_vpc.WNGNL_VPC.id
  tags   = { Name = "skills-igw" } # < IGW 태그 >
}

resource "aws_route_table" "WNGNL_PUBLIC_RT" { # 라우팅 테이블
  vpc_id                  = aws_vpc.WNGNL_VPC.id
  route {
    cidr_block            = "0.0.0.0/0"
    gateway_id            = aws_internet_gateway.WNGNL_IGW.id
  }
  tags   = { Name = "skills-pub-rt" } # < 라우팅 테이블 태그 >
}

# --- Create Public Subnet A ---
resource "aws_subnet" "WNGNL_PUBLIC_A" {
  vpc_id                 = aws_vpc.WNGNL_VPC.id
  cidr_block             = "10.0.1.0/24" # < Subnet CIDR >
  availability_zone      = "ap-northeast-2a" # < Subnet 리전 > 
  map_public_ip_on_launch = true
  tags = {  # < Sunbet 태그 >
    Name = "skills-pub-a"
    "kubernetes.io/role/elb" = "1"
  }
}
resource "aws_route_table_association" "WNGNL_PUBLIC_A_association" {
  subnet_id      = aws_subnet.WNGNL_PUBLIC_A.id
  route_table_id = aws_route_table.WNGNL_PUBLIC_RT.id
}

# --- Create Public Subnet B ---
resource "aws_subnet" "WNGNL_PUBLIC_B" {
  vpc_id                 = aws_vpc.WNGNL_VPC.id
  cidr_block             = "10.0.2.0/24" # < Subnet CIDR >
  availability_zone      = "ap-northeast-2b" # < Subnet 리전 > 
  map_public_ip_on_launch = true
  tags = { # < Sunbet 태그 >
    Name = "skills-pub-b" 
    "kubernetes.io/role/elb" = "1"
  } 
}
resource "aws_route_table_association" "WNGNL_PUBLIC_B_association" {
  subnet_id      = aws_subnet.WNGNL_PUBLIC_B.id
  route_table_id = aws_route_table.WNGNL_PUBLIC_RT.id
}

# --- Create Public Subnet C ---
resource "aws_subnet" "WNGNL_PUBLIC_C" {
  vpc_id                 = aws_vpc.WNGNL_VPC.id
  cidr_block             = "10.0.3.0/24" # < Subnet CIDR >
  availability_zone      = "ap-northeast-2c" # < Subnet 리전 > 
  map_public_ip_on_launch = true
  tags = { # < Sunbet 태그 >
    Name = "skills-pub-c"
    "kubernetes.io/role/elb" = "1"
  }
}
resource "aws_route_table_association" "WNGNL_PUBLIC_C_association" {
  subnet_id      = aws_subnet.WNGNL_PUBLIC_C.id
  route_table_id = aws_route_table.WNGNL_PUBLIC_RT.id
}



# --- Create Private Subnet A ---
resource "aws_eip" "WNGNL_EIP_FOR_NATGW_A" { # 탄력적 IP
  domain = "vpc"
}
resource "aws_nat_gateway" "WNGNL_NATGW_A" {
  allocation_id = aws_eip.WNGNL_EIP_FOR_NATGW_A.id
  subnet_id     = aws_subnet.WNGNL_PUBLIC_A.id
  tags = { Name = "nat-a" } # < NatGW 태그 >
}
resource "aws_route_table" "WNGNL_PRIVATE_A_RT" { # 프라이빗 라우팅 테이블
  vpc_id                  = aws_vpc.WNGNL_VPC.id
  route {
    cidr_block            = "0.0.0.0/0"
    gateway_id            = aws_nat_gateway.WNGNL_NATGW_A.id
  }
  tags   = { Name = "skills-priv-a-rt" } # < 라우팅 테이블 태그 >
}
resource "aws_subnet" "WNGNL_PRIVATE_A" { # 프라이빗 서브넷 생성
  vpc_id            = aws_vpc.WNGNL_VPC.id
  cidr_block        = "10.0.4.0/24" # < Subnet CIDR >
  availability_zone = "ap-northeast-2a" # < NATGW 리전 >
  tags = { # < Sunbet 태그 >
    Name = "skills-priv-a" 
    "kubernetes.io/role/internal-elb" = "1"
  } 
}
resource "aws_route_table_association" "WNGNL_PRIVATE_A_association" {
  subnet_id         = aws_subnet.WNGNL_PRIVATE_A.id
  route_table_id    = aws_route_table.WNGNL_PRIVATE_A_RT.id
}

# --- Create Private Subnet B ---
resource "aws_eip" "WNGNL_EIP_FOR_NATGW_B" { # 탄력적 IP
  domain = "vpc"
}
resource "aws_nat_gateway" "WNGNL_NATGW_B" {
  allocation_id = aws_eip.WNGNL_EIP_FOR_NATGW_B.id
  subnet_id     = aws_subnet.WNGNL_PUBLIC_B.id
  tags = { Name = "nat-b" } # < NatGW 태그 >
}
resource "aws_route_table" "WNGNL_PRIVATE_B_RT" { # 프라이빗 라우팅 테이블
  vpc_id                  = aws_vpc.WNGNL_VPC.id
  route {
    cidr_block            = "0.0.0.0/0"
    gateway_id            = aws_nat_gateway.WNGNL_NATGW_B.id
  }
  tags   = { Name = "skills-priv-b-rt" } # < 라우팅 테이블 태그 >
}
resource "aws_subnet" "WNGNL_PRIVATE_B" { # 프라이빗 서브넷 생성
  vpc_id            = aws_vpc.WNGNL_VPC.id
  cidr_block        = "10.0.5.0/24" # < Subnet CIDR >
  availability_zone = "ap-northeast-2b" # < NATGW 리전 >
  tags = { # < Sunbet 태그 >
    Name = "skills-priv-b" 
    "kubernetes.io/role/internal-elb" = "1"
  } 
}
resource "aws_route_table_association" "WNGNL_PRIVATE_B_association" {
  subnet_id         = aws_subnet.WNGNL_PRIVATE_B.id
  route_table_id    = aws_route_table.WNGNL_PRIVATE_B_RT.id
}

# --- Create Private Subnet C ---
resource "aws_eip" "WNGNL_EIP_FOR_NATGW_C" { # 탄력적 IP
  domain = "vpc"
}
resource "aws_nat_gateway" "WNGNL_NATGW_C" {
  allocation_id = aws_eip.WNGNL_EIP_FOR_NATGW_C.id
  subnet_id     = aws_subnet.WNGNL_PUBLIC_C.id
  tags = { Name = "nat-c" } # < NatGW 태그 >
}
resource "aws_route_table" "WNGNL_PRIVATE_C_RT" { # 프라이빗 라우팅 테이블
  vpc_id                  = aws_vpc.WNGNL_VPC.id
  route {
    cidr_block            = "0.0.0.0/0"
    gateway_id            = aws_nat_gateway.WNGNL_NATGW_C.id
  }
  tags   = { Name = "skills-priv-c-rt" } # < 라우팅 테이블 태그 >
}
resource "aws_subnet" "WNGNL_PRIVATE_C" { # 프라이빗 서브넷 생성
  vpc_id            = aws_vpc.WNGNL_VPC.id
  cidr_block        = "10.0.6.0/24" # < Subnet CIDR >
  availability_zone = "ap-northeast-2c" # < NATGW 리전 >
  tags = { # < Sunbet 태그 >
    Name = "skills-priv-c" 
    "kubernetes.io/role/internal-elb" = "1"
  } 
}
resource "aws_route_table_association" "WNGNL_PRIVATE_C_association" {
  subnet_id         = aws_subnet.WNGNL_PRIVATE_C.id
  route_table_id    = aws_route_table.WNGNL_PRIVATE_C_RT.id
}
```
