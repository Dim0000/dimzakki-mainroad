---
title: 【Hugo】Github ActionsでS3に自動でデプロイする【ブログ移行④】
description: 今回はHugoをGithubにプッシュした際に、自動でS3にデプロイする方法を紹介します。
date: 2023-11-24
categories: 
  - 技術記事
tags: 
  - ブログ運営
  - Hugo
  - AWS
  - GitHub
archives: 
  - 2023/11
thumbnail: /images/hugo.webp
---

今回は**Hugo**を**Github**にプッシュした際に、自動でS3にデプロイする方法を紹介します。

<!--more-->

必要な手順としては、IAMロールの設定と、Github Actionsの設定のみになります。

{{< box "関連記事" >}}
<ul>
<li>{{< ref "/wordpress-to-hugo" >}}</li>
<li>{{< ref "/domain-to-route53" >}}</li>
<li>{{< ref "/develop-hugo" >}}</li>
<li>{{< ref "/hugo-deploy" >}}</li>
</ul>
{{< /box >}}

## IAMロールの設定

まずは、IAMコンソールのIDプロバイダから、OIDCプロバイダの設定を以下の様に行います。プロバイダのタイプは`OpenID Connect`で、プロバイダのURLは`https://token.actions.githubusercontent.com`、対象者は`sts.amazonaws.com`を入力します。

{{< luminous src="/images/hugo-github-01.png" caption="OIDCプロバイダの設定">}}

続いてIAMロールを作成します。作成したロールの信頼ポリシーの編集で、ポリシーを以下の様に設定します。

{{< code lang="json" title="信頼ポリシー" >}}
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::<AWSアカウントID>:oidc-provider/token.actions.githubusercontent.com"
      },
      "Action": "sts:AssumeRoleWithWebIdentity",
      "Condition": {
        "StringLike": {
          "token.actions.githubusercontent.com:sub": "repo:<GitHubユーザー名>/<GitHubリポジトリ名>:*"
        }
      }
    }
  ]
}
{{< /code >}}

## Github Actionsの設定

続いて、Github Actionsの設定です。GitHubリポジトリの`.github/workflows/`ディレクトリに`deploy.yml`を配置します。Hugoのビルドは「[peaceiris/actions-hugo](https://github.com/peaceiris/actions-hugo)」を使用します。

{{< code lang="yml" title="deploy.yml" >}}
name: deploy

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: true
          fetch-depth: 0 # enableGitInfoでの取得用
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: "latest"
          extended: true     
      - name: Build Hugo
        run: hugo --minify --buildFuture
      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-region: ap-northeast-1
          role-to-assume: arn:aws:iam::${{ secrets.AWS_ACCOUNT_ID }}:role/blog_github_action_role
          role-session-name: GitHubActions-${{ github.run_id }}
          role-duration-seconds: 900
      - name: Upload files to the production website with the AWS CLI
        run: |
          echo "uploding to s3 ..."
          aws s3 sync public s3://${{ secrets.S3_BUCKET }}/ --size-only --delete
          aws cloudfront create-invalidation --region ap-northeast-1 --distribution-id ${{ secrets.DISTRIBUTION_ID }} --paths "/*"
{{< /code >}}

また、GitHubのSecrets設定で、`AWS_ACCOUNT_ID`にAWSのアカウントID、`S3_BUCKET`にS3バケット名、`DISTRIBUTION_ID`にはディストリビューションIDを設定します。

{{< luminous src="/images/hugo-github-02.png" caption="GitHubのSecrets設定">}}

これで設定は完了ですので、実際にGitHubにプッシュし、All workflowsの画面から処理が実行されているのを確認できたら成功です。

{{< luminous src="/images/hugo-github-03.png" caption="GitHubのAll workflows画面">}}

* * *

これでプッシュ時に自動でS3にデプロイできるようになりました。ここまでで、Hugoブログの構築が完了となります。以上で記事を終わりにします。

## 参考文献

* [GitHub ActionsにAWSクレデンシャルを直接設定したくないのでIAMロールを利用したい | DevelopersIO](https://dev.classmethod.jp/articles/github-actions-aws-sts-credentials-iamrole/)

* [静的サイトジェネレータ「Hugo」でブログサイトを S3 + CloudFrontでホストするまで [第4回〜HugoをGithub Action でS3にプッシュする〜] | 犬と暮らすエンジニア日記](https://technowanko.com/posts/hugo/004_hugo_build_on_github/)