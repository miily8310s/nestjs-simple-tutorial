# Nest.js 開発事始めメモ

## Nest.js とは

Nest.js とは Node.js のサーバーサイドフレームワークです。TypeScript（以下 TS）での開発がメインとなりますが、JavaScript（以下 JS）での開発も可能です。
[Express](https://expressjs.com/)（デフォルト）もしくは[Fastify](https://www.fastify.io/)（オプション）といった HTTP サーバーフレームワークをコアに動作する仕組みとなっており、実際これらの HTTP サーバーフレームワークからメソッド等を`import`してきて使うことも可能です。

https://docs.nestjs.com/

アーキテクチャは[Angular](https://angular.jp/)に強い影響を受けており、デコレーターを活用したコードの記述や開発ルールの強制の仕方/実装と疎結合になるディレクトリ構成にはその影響が伺えます。ただこうした硬めな印象を受けるアーキテクチャのアプローチは、これまで決定打がなく問題となっていた Node.js/サーバーサイド JS 開発のアーキテクチャの難しさを解決するものとなっております。

こうした TS フレンドリーかつアーキテクチャ設計が受け入れられたのか、国内でもここ 1 年の間に採用事例が増えており、今後も増えることが予想されます。

https://engineering.mercari.com/blog/entry/20210818-mercari-shops-nestjs-graphql-server/

https://www.forcia.com/blog/002380.html

https://techblog.yahoo.co.jp/entry/2020121530052952/

また、その他の特徴としては以下が挙げられます。

- RESTAPI だけではなく、もちろん GraphQL サーバーの開発も可能
- Nest Cli で簡単にアプリやファイルのテンプレートを作成出来る
- テスト用フレームワークがすでに用意されており、疎結合な構成のおかげでテストコードの作成が容易
- OpenAPI（Swagger）のサポート（要プラグイン）
- 豊富な公式ドキュメントとサンプル集

ちなみに公式サイトのハンバーガーメニューが本当にハンバーガーになっている点は個人的にすごく好きです。

## 開発環境

MacBook Pro(M1 2020)
macOS Monterey 12.1

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

メインは src フォルダ
root 直下には E2E テスト用の test フォルダもいるが、今回は触れない。

```md
src/
|- app.controller.spec.ts
|- app.controller.ts
|- app.module.ts
|- app.service.ts
|- main.ts
```

では一つずつ説明していく。

https://docs.nestjs.com/first-steps#setup

まず Nest.js でサーバーを立ち上げる上で以下の 4 つのファイルが必須となります。

- main
- controllers
- providers
- modules

これらの役割を先程の`src/`直下のファイルと紐付けながら説明していきます。

## main.ts

main の役割を司るのがこのファイルです。
Nest.js アプリのエントリーポイントとなり、サーバのインスタンスを作成＆サーバーの起動はこのファイルからのみ行います。

[Express](https://expressjs.com/)を扱ったことがある人には`const app = express()`〜`app.listen()`と同じことしているなぁって思ってもらえば大丈夫です。

```ts: src/main.ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  // NestFactoryというFunctionでNest.jsアプリのインスタンスを生成
  const app = await NestFactory.create(AppModule);
  // 3000番portでサーバーを立ち上げる
  await app.listen(3000);
}
bootstrap();
```

## app.controller.ts / app.controller.spec.ts

`controllers`の役割を司るファイルです。サーバーのルーティングを行う箇所です。
初期コードでは`@Get()`デコレーターを使用し、GET で`[rootURL]/`を叩いたとき呼び出される`getHello`メソッドが定義されています。

今回は触れませんが、いわゆるリダイレクトや認証確認などのガード処理もここで設定します。
`AppService`については後で説明します。

```ts: src/app.controller.ts
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }
}
```

なお、`app.controller.spec.ts`は JS/TS のユニットテスト作成ライブラリである[Jest](https://jestjs.io/ja/) を使って、`app.controller.ts`のテストコードを作成する箇所です。

## app.service.ts

`providers`の役割を司るファイルです。`controllers`の処理を詳細に定義する場所だと思ってもらえば大丈夫です。

今回は詳しく触れませんが、ここでは`@Injectable()`デコレーターを使用しており、DI（Dependency Injection/依存性注入）の仕組みが使用されています。

DI についてもっと知りたい方はこちらの資料を読んだみてください。
https://qiita.com/hinom77/items/1d7a30ba5444454a21a8
https://qiita.com/ritukiii/items/de30b2d944109521298f

```ts: src/app.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class AppService {
  // APIが呼び出されたときに'Hello World!'を返却するメソッドを定義
  getHello(): string {
    return 'Hello World!';
  }
}
```

## app.module.ts

`modules`の役割を司るファイルです。`controllers` とか `providers` を取りまとめるところです。

```ts: src/app.module.ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}

```

# 今後したいこと

デコレーターや Angular ライクな書き方にまだまだ慣れていないので、まずは公式の Overview の全ページを一周しようと思っています。ずっと勉強できてなかった TypeORM の活用 や GraphQL 実装あたりにも踏み込めればと思っています。

# 参考にしたページ

https://docs.nestjs.com/first-steps

https://zenn.dev/hakushun/articles/0b0443ac6fd8d7

https://qiita.com/elipmoc101/items/9b1e6b3efa62f3c2a166
