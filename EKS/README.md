Eks Cluster IAM Role with awscli
```
# Eks-Cluster-Role IAM 역할 생성
aws iam create-role --role-name Eks-Cluster-Role --assume-role-policy-document '{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {
      "Service": "eks.amazonaws.com"
    },
    "Action": "sts:AssumeRole"
  }]
}'
# AmazonEKSClusterPolicy 정책을 Eks-Cluster-Role에 연결
aws iam attach-role-policy --role-name Eks-Cluster-Role --policy-arn arn:aws:iam::aws:policy/AmazonEKSClusterPolicy
```

Eks NodeGroup IAM Role with awscli
```
# Eks-NodeGroup-Role IAM 역할 생성
aws iam create-role --role-name Eks-NodeGroup-Role --assume-role-policy-document '{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {
      "Service": "ec2.amazonaws.com"
    },
    "Action": "sts:AssumeRole"
  }]
}'
# AdministratorAccess 정책을 Eks-NodeGroup-Role에 연결
aws iam attach-role-policy --role-name Eks-NodeGroup-Role --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
# AmazonEC2ContainerRegistryReadOnly 정책을 Eks-NodeGroup-Role에 연결
aws iam attach-role-policy --role-name Eks-NodeGroup-Role --policy-arn arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryReadOnly
# AmazonEKSWorkerNodePolicy 정책을 Eks-NodeGroup-Role에 연결
aws iam attach-role-policy --role-name Eks-NodeGroup-Role --policy-arn arn:aws:iam::aws:policy/AmazonEKSWorkerNodePolicy
# AmazonEKS_CNI_Policy 정책을 Eks-NodeGroup-Role에 연결
aws iam attach-role-policy --role-name Eks-NodeGroup-Role --policy-arn arn:aws:iam::aws:policy/AmazonEKS_CNI_Policy
```
