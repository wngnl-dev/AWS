# 현재 날짜
```
version: 0.2

phases:
  pre_build:
    commands:
      # CodeCommit 저장소에서 소스 코드를 가져옵니다.
      - echo "Cloning source code from CodeCommit..."
      - <클라이언트 인증>
      - MY_ECR=<ECR URI>
      - docker_img_name=$(TZ=Asia/Seoul date '+%Y%m%d%H%M%S')

  build:
    commands:
      # Docker 이미지 빌드
      - echo "Building Docker image..."
      - docker build -t $docker_img_name .
  post_build:
    commands:
      # Docker 이미지를 Amazon ECR에 푸시
      - echo "Pushing Docker image to Amazon ECR..."
      - docker tag $docker_img_name:latest $MY_ECR:<태그>
      - docker push $MY_ECR:<태그>  # 이미지를 ECR에 푸시합니다

```

<br>

# 커밋 ID
```
version: 0.2
phases:
  pre_build:
    commands:
      - <ECR Aws Cli 인증>
      - ECR_URL=885074404314.dkr.ecr.ap-northeast-2.amazonaws.com/test-ecr
      - COMMIT_ID=$(echo $CODEBUILD_RESOLVED_SOURCE_VERSION | cut -c 1-7)
      - echo $COMMIT_ID
      - LATEST=${COMMIT_ID:=latest}
  build:
    commands:
      - docker build -t $ECR_URL:$LATEST .
  post_build:
    commands:
      - docker push $ECR_URL:$LATEST
```
