# Terraform 설치 < CMD 관리자 권한으로 실행 >
Chocolatey 설치 < CMD >
```
@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```
Chocolatey 설치 < Powershell >
```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

<br>

## terraform 설치
```
choco install terraform
```
## Windows Aws Cli V2 설치
```
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi /qn
```

<br>

provider.tf 파일로 Terraform 기본 설정을 해줍니다.
```
terraform init
```

Terraform 파일을 참고해서 컴파일 해줍니다.
```
terraform plan
```

Terraform 파일을 참고하여 리소스를 생성합니다.
```
terraform apply
```
 
Terraform 파일을 참고해서 리소스를 삭제합니다.
```
terraform destroy
```




