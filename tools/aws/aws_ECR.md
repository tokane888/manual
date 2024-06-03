# aws ECR

## メモ

- 下記2つは同じ意味らしい
  - EC2 Container registry
  - Elastic Container registry

## エラー対応

```
 ~/learn/aws/ecr aws ecr get-login-password

An error occurred (AccessDeniedException) when calling the GetAuthorizationToken operation: User: arn:aws:iam::436638440860:user/test2 is not authorized to perform: ecr:GetAuthorizationToken on resource: * because no identity-based policy allows the ecr:GetAuthorizationToken action
```

- 上記エラーは下記ポリシーattachで解消
  - AmazonEC2ContainerRegistryFullAccess
