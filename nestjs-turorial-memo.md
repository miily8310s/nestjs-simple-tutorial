# Nest.js 開発事始めメモ

## インストール

https://docs.nestjs.com/first-steps#setup

```bash
# npmの場合
# npm i -g @nestjs/cli

yarn global add @nestjs/cli
which nest
# /Users/hogehoge/.yarn/bin/nest と返却された

# nest new <プロジェクト名> で生成可能
nest new nest new nestjs-simple-tutorial

⚡  We will scaffold your app in a few seconds..

CREATE nestjs-simple-tutorial/.eslintrc.js (631 bytes)
CREATE nestjs-simple-tutorial/.prettierrc (51 bytes)
CREATE nestjs-simple-tutorial/README.md (3339 bytes)
CREATE nestjs-simple-tutorial/nest-cli.json (64 bytes)
CREATE nestjs-simple-tutorial/package.json (2011 bytes)
CREATE nestjs-simple-tutorial/tsconfig.build.json (97 bytes)
CREATE nestjs-simple-tutorial/tsconfig.json (546 bytes)
CREATE nestjs-simple-tutorial/src/app.controller.spec.ts (617 bytes)
CREATE nestjs-simple-tutorial/src/app.controller.ts (274 bytes)
CREATE nestjs-simple-tutorial/src/app.module.ts (249 bytes)
CREATE nestjs-simple-tutorial/src/app.service.ts (142 bytes)
CREATE nestjs-simple-tutorial/src/main.ts (208 bytes)
CREATE nestjs-simple-tutorial/test/app.e2e-spec.ts (630 bytes)
CREATE nestjs-simple-tutorial/test/jest-e2e.json (183 bytes)

# どのパッケージ管理ツールを使うかの質問が来る
Which package manager would you ❤️  to use?
  npm
❯ yarn
  pnpm


🚀  Successfully created project nestjs-simple-tutorial
👉  Get started with the following commands:

$ cd nestjs-simple-tutorial
$ yarn run start


                          Thanks for installing Nest 🙏
                 Please consider donating to our open collective
                        to help us maintain this package.


               🍷  Donate: https://opencollective.com/nest


$ cd nestjs-simple-tutorial && code .
```

## 初期生成されたテンプレートについて

サーバーの立ち上げ

```bash
yarn run start
# 叩くと `dist/`フォルダが生成される

yarn run v1.22.17
$ nest start
[Nest] 23190  - 2022/03/01 8:38:00     LOG [NestFactory] Starting Nest application...
[Nest] 23190  - 2022/03/01 8:38:00     LOG [InstanceLoader] AppModule dependencies initialized +30ms
[Nest] 23190  - 2022/03/01 8:38:00     LOG [RoutesResolver] AppController {/}: +2ms
[Nest] 23190  - 2022/03/01 8:38:00     LOG [RouterExplorer] Mapped {/, GET} route +1ms
[Nest] 23190  - 2022/03/01 8:38:00     LOG [NestApplication] Nest application successfully started +1ms
```

http://localhost:3000/ へアクセスすると、Hello World!　の文字が！

ディレクトリ階層について
