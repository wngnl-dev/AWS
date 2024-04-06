# Lambda Boto3로 EC2 Instance 생성하는 코드

```
import boto3

aws = boto3.Session()
ec2 = aws.resource('ec2', region_name="<>") # 현재 리전 이름


tags = [{'Key':'Name','Value': '<bastion-ec2>'}]

ec2_user_data = '''
#!/bin/bash
'''

instance = ec2.create_instances(
    IamInstanceProfile={'Name': "IAM 역활 이름"}, # IAM 역활 이름
    ImageId='<AMI Image ID>', # AMI Image ID
    InstanceType='t2.micro', # 인스턴스 타입

    UserData = f"{ec2_user_data}", # 사용자 데이터
	TagSpecifications = [{'ResourceType': 'instance', 'Tags': tags}], # 태그
    MinCount=1, # 최소
    MaxCount=1, # 최대
    
    NetworkInterfaces=[{
        'SubnetId': "<>", # 서브넷 ID
        'DeviceIndex': 0,
        'Groups':["<>"] # 보안그룹 ID
    }]
)

print(instance)
```
