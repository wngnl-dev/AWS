# Codebuild에서 Build를 생성할때
"환경" - "도커 이미지를 빌드하거나 빌드의 권한을 승격하려면 이 플래그를 활성화합니다."
을 꼭 활성화 해줘야 합니다.

<br>

## ECS Service 강제 업데이트 하기
```
aws ecs update-service --cluster <ECS cluster 이름> --service <SVC 이름> --force-new-deployment
```
## ECR 이미지 자동으로 스캔한기
```
aws ecr start-image-scan --repository-name <ECR 이름> --image-id imageTag=<ECR Img 태그>
```
