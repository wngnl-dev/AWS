wget https://raw.githubusercontent.com/wngnl-dev/AWS/main/CICD/EC2_Deploy/Dockerfile
wget https://raw.githubusercontent.com/wngnl-dev/AWS/main/CICD/EC2_Deploy/appspec.yml
wget https://raw.githubusercontent.com/wngnl-dev/AWS/main/CICD/EC2_Deploy/buildspec.yml
mkdir script
wget -O script/ec2_deploy.sh https://raw.githubusercontent.com/wngnl-dev/AWS/main/CICD/EC2_Deploy/script/ec2_deploy.sh
