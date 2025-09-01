# Nuxt4 Template

Nuxt を簡単にインストールしてすぐ使うためのテンプレート

- NOLICENSED ご自由にお使いください

## 前提

Node.js と npm と yarn くらい入ってるよね！（投げやり）
デプロイ先は Vercel を想定してるけど多分どこでも動きます
あと PHP の composer も用意してね

## INCLUDED

- Vue CLI Service
- Vue3
- Vuetify3
- Vuetify ダークテーマ
- Nuxt4
- Vue-router
- VSCode、Git、Eslint、Prettier 周りの設定ファイル
- Pug と SASS
- PWA Preset
- Google Fonts
- Vue Content Loader

## 独自実装

- Cookie API
- Ajax API
- 画面を右スワイプでメニュー表示
- イイカンジにカスタマイズされた SCSS ファイル
- コピペで使える pug テンプレート
- 汎用性の高い関数群
- ダークテーマ切り替えボタン
- Push API（使いやすいように改良）
- Notification API（使いやすいように改良）
- アカウント登録時のメールアドレス認証、アクセストークンの発行
- MySQL 用 API

## 制作予定

- リッチエディタ

## 注意

ポート 12345 で動くようにしてあります  
VSCode での利用を推奨

~~Vue3 慣れてなくて Options API 使ってるけど許して~~

## 参考資料

WebPush <https://tech.excite.co.jp/entry/2021/06/30/104213>

## Setup

このプログラムは、表示用サーバーと処理用サーバーの 2 つが必要です

### 表示用サーバー

```shell
git clone git@github.com:jikantoki/nuxt3temp.git
echo 'これだけでセットアップ完了！'
echo 'Vercelとかでデプロイしたらそのまま動く'
```

### WebPush 用の鍵を作成

ここで作れます <https://web-push-codelab.glitch.me/>

#### ストレージを操作できる環境の場合

ルートに.env ファイルを作成し、以下のように記述（クォーテーション不要）

```env
VUE_APP_WEBPUSH_PUBLICKEY=パブリックキーをコピー
VUE_APP_WEBPUSH_PRIVATEKEY=プライベートキーをコピー

VUE_APP_API_ID=default
VUE_APP_API_TOKEN=後のPHPで作成するアクセストークン
VUE_APP_API_ACCESSKEY=後のPHPで作成するアクセスキー

VUE_APP_API_HOST=APIサーバーのホスト
```

#### それ以外（Vercel デプロイ等）

Project Settings → Enviroment Variables を開く  
上記.env ファイルと同じ感じで設定

### PHP サーバー（内部処理用）

サーバーサイドは PHP で開発しているため、一部処理を実行するには PHP サーバーの用意が必要です  
とりあえずレンタルサーバーでも借りれば実行できます

1. API 用のドメインをクライアント側（Vercel 等）とは別で用意する
2. このリポジトリの php フォルダをドメインのルートにする（.htaccess 等で）
3. （準備中！！！）に API 用のドメインを記述
4. リポジトリルート直下に/env.php を用意し、以下の記述をする

```php
<?php
define('DIRECTORY_NAME', '/プロジェクトルートのディレクトリ名');

define('VUE_APP_WebPush_PublicKey', 'パブリックキー');
define('VUE_APP_WebPush_PrivateKey', 'プライベートキー');
define('WebPush_URL', 'プッシュ通知を使うドメイン');
define('WebPush_URL_dev', 'プッシュ通知を使うドメイン（開発用）');//この行は無くても良い
define('WebPush_icon', 'プッシュ通知がスマホに届いたときに表示するアイコンURL');
define('Default_user_icon', 'アイコン未設定アカウント用の初期アイコンURL');

define('MySQL_Host', 'MySQLサーバー');
define('MySQL_DBName', 'DB名');
define('MySQL_User', 'DB操作ユーザー名');
define('MySQL_Password', 'DBパスワード');

define('SMTP_Name', '自動メール送信時の差出名');
define('SMTP_Username', 'SMTPユーザー名');
define('SMTP_Mailaddress', '送信に使うメールアドレス');
define('SMTP_Password', 'SMTPパスワード');
define('SMTP_Server', 'SMTPサーバー');
define('SMTP_Port', 587); //基本は587を使えば大丈夫

```

#### PHP サーバー用の.htaccess の用意

大体こんな感じで設定する

```htaccess
#トップページを/nuxt3temp/php にする
<IfModule mod_rewrite.c>
RewriteEngine on
RewriteBase /
RewriteRule ^$ nuxt3temp/php/ [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.+)$ nuxt3temp/php/$1 [L]
</IfModule>
# 外部からのAPIへのアクセスを許可
Header append Access-Control-Allow-Origin: "*"

```

### MySQL の用意

#### /database.sql ファイルをインポートする

PHPMyAdmin が使える環境なら DB 直下にインポートして終わり、コマンドラインでやる方法は知らん

#### ※インポートでエラーが出たら

/database_VIEW.sql の中身をコピーして phpmyadmin で直接実行

### デフォルト API のトークンを用意する

このプログラムは独自のアクセストークンを利用して API にアクセスします。  
そのため、初回 API を登録する作業が必要です。

1. セットアップした API 用サーバーの/makeApiForAdmin.php にアクセス
2. 初回アクセス時のみ MySQL で登録作業が行われるので、出てきた画面の内容をコピー
3. .env にｲｲｶﾝｼﾞに内容を記述（書き方はさっき説明した）
4. 以後、その値を使って API を操作できます

**忘れたらリセット**するしかないので注意！（一部データは暗号化されており、管理者でも確認できません）

#### デフォルト API トークンのリセット方法

1. MySQL の api_list テーブルの secretId='default'を削除
2. api_listForView の secretId='default'も同様に削除
3. 初回登録と同じ感じでやる
4. データベースに再度 default が追加されていることを確認

## コンソール側で初期化

```shell
yarn install
composer install #PHP用
```

### 実行

```shell
yarn run dev
```

### 設定方法

| 項目           | 設定箇所                     |
| -------------- | ---------------------------- |
| アプリ名       | /package.json                |
| フォント       | /layout/default.vue          |
| ナビゲーション | /items/itemNavigationList.js |
| 404 ページ     | /error.vue                   |

### Compiles and minifies for production

```shell
yarn build
```

### Lints and fixes files

```shell
yarn lint
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).

## トラブルシューティング

### PHP がおかしい

composer ちゃんと入れた？

## Nuxt Minimal Starter

Look at the [Nuxt documentation](https://nuxt.com/docs/getting-started/introduction) to learn more.

## Development Server

Start the development server on `http://localhost:3000`:

```bash
# npm
npm run dev

# pnpm
pnpm dev

# yarn
yarn dev

# bun
bun run dev
```

## Production

Build the application for production:

```bash
# npm
npm run build

# pnpm
pnpm build

# yarn
yarn build

# bun
bun run build
```

Locally preview production build:

```bash
# npm
npm run preview

# pnpm
pnpm preview

# yarn
yarn preview

# bun
bun run preview
```

Check out the [deployment documentation](https://nuxt.com/docs/getting-started/deployment) for more information.
