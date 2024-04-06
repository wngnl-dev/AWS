## Down Codecommit
```
git clone <code commit https>
```
<br>

## Create Branch
```
git branch -M <브런치 이름>
```
```
git checkout -b <브런치 이름>
```
<br>

## Switch Branch
```
git switch <브런치 이름>
```
<br>

## Pull Branch
```
git pull origin <브런치>
```
<br>

## Push Branch
```
git add .
git commit -m "<업데이트 내용>"
git push origin <브런치 이름>
```
<br>

## Git 자격증명
```
git config --global credential.helper store
git pull
```

## Git 자격증명 초기화
```
git config --global credential.helper '!aws codecommit credential-helper $@'
git config --global credential.UseHttpPath true
```


