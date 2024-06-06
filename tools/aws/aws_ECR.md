# aws ECR

## メモ

- 下記2つは同じ意味らしい
  - EC2 Container registry
  - Elastic Container registry

## 参考

- 公式tutorial
  - https://docs.aws.amazon.com/AmazonECR/latest/userguide/getting-started-cli.html

## エラー対応

```
 ~/learn/aws/ecr aws ecr get-login-password

An error occurred (AccessDeniedException) when calling the GetAuthorizationToken operation: User: arn:aws:iam::436638440860:user/test2 is not authorized to perform: ecr:GetAuthorizationToken on resource: * because no identity-based policy allows the ecr:GetAuthorizationToken action
```

- 上記エラーは下記ポリシーattachで解消
  - AmazonEC2ContainerRegistryFullAccess

## image登録手順

- Dockerfile作成
- build
  - docker build -t hello-world .
- コンテナimage動作確認
  - docker run -t -i -p 80:80 hello-world
- registryのtoken取得確認
  - aws ecr get-login-password
  - failする場合はAmazonEC2ContainerRegistryFullAccessポリシー等attach済みか確認
- token取得+docker login
  - aws ecr get-login-password | docker login --username AWS --password-stdin (repository URL)
    - usernameはAWSで固定
- repository作成
  - aws ecr create-repository --repository-name hello-repository --region ap-northeast-1
- repository一覧確認
  - aws ecr describe-repositories
- docker tag付与
  - docker tag hello-world:latest `aws_account_id`.dkr.ecr.`region`.amazonaws.com/hello-repository
- docker image push
  - docker push `aws_account_id`.dkr.ecr.`region`.amazonaws.com/hello-repository
- repositoryに登録されているimage一覧表示
  - aws ecr describe-images --repository-name (repository名)
- docker image pull
  - docker pull `aws_account_id`.dkr.ecr.`region`.amazonaws.com/hello-repository:latest
