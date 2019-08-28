# Serverless Frameworkメモ

## サービスの雛形を作成する

Node.jsのサーバレスサービスを新規開発する場合、以下のコマンドを実行する。

```terminal
serverless create --template aws-nodejs --path serverless-prj
```

作成に成功すると `serverless-prj` フォルダが作られ、フォルダの中に `handler.js` と `serverless.yml` ができる。

### `handler.js`

エントリポイントになるNode.js。

### `serverless.yml`

Lambda, API Gateway, DynamoDBなどの設定を書くYAML。


## Lambdaをローカルでテストする

`serverless invoke` で `handler.js` のファンクションを実行できる。

```terminal
serverless invoke local --function function-name
```

`local` オプションを外すとデプロイされたLambdaを実行する。


## ローカルでAPI Gateway+Lambda+DynamoDBの構成をテストする

デプロイする前に `serverless.yml` の設定でサービスをテストする場合、次のプラグインを `npm` でインストールし、`serverless.yml` に追加する。

### `serverless-offline`

API Gatewayの代替になるWebサーバを起ち上げ、WebAPIからLambdaを呼び出す

### `serverless-dynamodb-local`

DynamoDB LocalをServerless Frameworkで使えるようにする。


## サービスをデプロイする

`serverless.yml` の設定でサービスをデプロイする。

`stage` オプションでデプロイする環境を指定できる。指定が無い場合、`serverless.yml` の `custom.defaultStage` の値を設定する。

```terminal
serverless deploy --stage production
```

## 自動デプロイ

CodeCommit + CodePipeline + CodeBuildで `git push` から自動デプロイできる。
GitLabからCodePipelineを直接連携できないようなのでCodeCommitを間に挟む必要がある？
