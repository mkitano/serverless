# Serverless Framework

## やりたいこと

+ `git push` で自動デプロイする
+ 自動でなくてもローカルからコマンドでデプロイできるようにしたい
+ 開発したLambdaのテスト環境を開発者ごとに作成する、テストが終わったら作成したテスト環境を削除する

## Node.jsのサーバレスサービスの雛形を作成

```
serverless create --template aws-nodejs --path serviceName
```

+ 成功すると `serverless-service` ディレクトリが作られ、`handler.js`, `serverless.yml` ができる
+ `handler.js` にエントリポイントとなるLambdaを実装する
+ `serverless.yml` に使用するAWSサービスの設定を書く


## ローカルでLambdaをテスト

```
serverless invoke local --function funcName
```

+ Node.jsのファンクションのテストをする場合、テストコードなどファンクションを呼び出すJSファイルが必要になるので、コマンドでファンクションを呼び出せるのが便利
+ `local` オプションを外すとデプロイされたLambdaをテストできるので、管理コンソールからの動作テストは不要


## ローカルでサービスをテストする

```
serverless offline
```

+ デプロイ前に `serverless.yml` の設定をテストできる
+ Serverless Frameworkプラグインの `serverless-offline`, `serverless-dynamodb-local` を入れれば、API Gateway, DynamoDBもローカルで動かすことができる


## サービスをデプロイする

```
serverless deploy --stage prod
```

+ `stage` オプションでデプロイの環境を指定できる
+ 指定がない場合、`serverless.yml` の `custom.defaultStage` の値を設定する

## サービスを削除する

```
serverless remove --stage dev
```

+ デプロイ済みのサービスを全削除するので定時後に定期実行できるといい


## Lambdaのバージョンとエイリアス設定

+ `serverless-aws-alias` プラグインを使えばいける？

## 自動デプロイ

### CodeCommit + CodePipeline + CodeBuild

+ CodeCommit + CodePipeline + CodeBuildを使用して `git push` をトリガーに自動デプロイできる
+ GitLabだとCodePipelineと直に連携できないので、CodeCommitを間に置いてミラーリングする必要がある？


### GitLab Runner

+ `git push` をトリガーにGitLab Runnerから `serverless deploy` を実行する
