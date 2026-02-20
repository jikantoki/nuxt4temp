# Nuxt4 Template

Nuxt を簡単にインストールしてすぐ使うためのテンプレート

- NOLICENSED ご自由にお使いください

## このプロジェクトについて

Nuxt4 をベースにした Web アプリ開発用テンプレートです。  
フロントエンド（Nuxt4 + Vuetify3）とバックエンド（PHP + MySQL）の 2 サーバー構成になっており、  
アカウント管理・WebPush 通知・Ajax 通信などの機能をあらかじめ実装しています。

デプロイ先は Vercel を想定していますが、他の環境でも動作します。

## 前提

- Node.js（LTS 推奨）
- yarn（パッケージマネージャー）
- PHP + Composer（バックエンド機能を使う場合）
- MySQL（データベース機能を使う場合）

## 含まれるライブラリ

| ライブラリ                       | 用途                               |
| -------------------------------- | ---------------------------------- |
| Nuxt 4                           | フレームワーク本体                 |
| Vue 3                            | UI フレームワーク                  |
| Vuetify 3                        | UI コンポーネント（ダークテーマ対応） |
| Vue Router                       | ルーティング                       |
| Pinia                            | 状態管理                           |
| pinia-plugin-persistedstate      | Pinia の状態永続化                 |
| vue-i18n                         | 多言語対応（日本語・英語）         |
| Pug                              | HTML テンプレートエンジン          |
| SASS                             | CSS プリプロセッサ                 |
| Google Fonts（Zen Maru Gothic）  | フォント                           |
| Material Design Icons（@mdi/font）| アイコン                          |
| Vue Content Loader               | スケルトンローディング             |
| VSCode / Git / Prettier 設定ファイル | 開発環境設定                  |

## 独自実装

- Ajax 通信用 API（`/app/js/ajaxFunctions.js`）
- WebPush 通知（Push API・Notification API を使いやすいようにラップ）
- アカウント登録・ログイン・パスワードリセット・メールアドレス認証
- アクセストークン発行による API 認証
- MySQL 操作用 PHP API
- SCSS カスタマイズ済みスタイル（レスポンシブ対応）
- Pinia によるユーザー状態管理
- ダークテーマ切り替え
- PWA 対応（manifest.json）

## 注意

- 開発サーバーはポート **12345** で起動します
- VSCode での利用を推奨します

## ファイル構成

```
nuxt4temp/
├── app/                         # Nuxt4 アプリケーション本体
│   ├── assets/                  # 画像・SCSS などの静的ファイル
│   │   └── main.scss            # グローバルスタイル
│   ├── components/              # Vue コンポーネント
│   │   └── common/              # 共通コンポーネント（ヘッダー、フッター等）
│   ├── composables/             # Composable（状態管理含む）
│   │   └── states.ts            # Pinia ストア定義
│   ├── items/                   # 設定リスト等
│   │   └── itemNavigationList.js # ナビゲーション項目
│   ├── js/                      # ユーティリティ関数群
│   │   ├── Functions.js         # 汎用関数
│   │   ├── ajaxFunctions.js     # Ajax 通信
│   │   ├── metaFunctions.js     # メタ情報操作
│   │   ├── setup.js             # 初期化処理
│   │   └── webpush.js           # WebPush 通知
│   ├── layouts/
│   │   └── default.vue          # 共通レイアウト（フォント設定含む）
│   ├── locales/                 # 多言語ファイル
│   │   ├── ja.js                # 日本語
│   │   └── en.js                # 英語
│   ├── mixins/                  # Vue ミックスイン
│   ├── pages/                   # ページコンポーネント
│   │   ├── index.vue            # トップページ
│   │   ├── login.vue            # ログイン
│   │   ├── registar.vue         # アカウント登録
│   │   ├── password_reset.vue   # パスワードリセット
│   │   ├── [userId].vue         # ユーザープロフィール
│   │   ├── about.vue            # About ページ
│   │   ├── rule.vue             # 利用規約
│   │   └── vuetify.vue          # Vuetify コンポーネント一覧
│   ├── plugins/                 # Nuxt プラグイン
│   └── error.vue                # 404・エラーページ
├── php/                         # PHP バックエンド
│   ├── functions/               # PHP 共通関数
│   ├── makeApiForAdmin.php      # API トークン初回発行
│   ├── createAccount.php        # アカウント作成
│   ├── loginAccount.php         # ログイン
│   ├── authAccount.php          # 認証
│   ├── resetPassword.php        # パスワードリセット
│   ├── sendPushForAccount.php   # WebPush 送信
│   └── ...                      # その他 API エンドポイント
├── public/                      # 公開静的ファイル（favicon 等）
├── database.sql                 # MySQL テーブル定義
├── database_VIEW.sql            # MySQL ビュー定義
├── nuxt.config.ts               # Nuxt 設定ファイル
└── package.json                 # 依存パッケージ定義
```

## セットアップ

このプログラムは、**フロントエンドサーバー**と**バックエンド（PHP）サーバー**の 2 つが必要です。

### 1. リポジトリのクローン

```shell
git clone git@github.com:jikantoki/nuxt4temp.git
cd nuxt4temp
```

### 2. 依存パッケージのインストール

```shell
yarn install
composer install  # PHP バックエンドを使う場合
```

### 3. 環境変数の設定

#### ローカル環境の場合

ルートに `.env` ファイルを作成し、以下のように記述（クォーテーション不要）

```env
VUE_APP_WEBPUSH_PUBLICKEY=パブリックキー
VUE_APP_WEBPUSH_PRIVATEKEY=プライベートキー

VUE_APP_API_ID=default
VUE_APP_API_TOKEN=PHPで発行するアクセストークン
VUE_APP_API_ACCESSKEY=PHPで発行するアクセスキー

VUE_APP_API_HOST=APIサーバーのホスト
```

#### Vercel デプロイの場合

Project Settings → Environment Variables を開き、上記と同様の内容を設定してください。

### 4. WebPush 用の鍵を作成（WebPush を使う場合）

<https://web-push-codelab.glitch.me/> でパブリックキーとプライベートキーを生成し、  
環境変数に設定してください。

### 5. PHP サーバーのセットアップ（バックエンドを使う場合）

1. API 用のドメインをフロントエンド（Vercel 等）とは別に用意する
2. `php/` フォルダをドメインのルートに配置する（`.htaccess` 等で設定）
3. リポジトリルート直下に `env.php` を作成し、以下の内容を記述する

```php
<?php
define('DIRECTORY_NAME', '/プロジェクトルートのディレクトリ名');

define('VUE_APP_WebPush_PublicKey', 'パブリックキー');
define('VUE_APP_WebPush_PrivateKey', 'プライベートキー');
define('WebPush_URL', 'プッシュ通知を使うドメイン');
define('WebPush_URL_dev', 'プッシュ通知を使うドメイン（開発用）'); // この行は省略可
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
define('SMTP_Port', 587); // 基本は587を使えば大丈夫
```

#### PHP サーバー用の `.htaccess` の例

```htaccess
# トップページを /nuxt4temp/php にする
<IfModule mod_rewrite.c>
RewriteEngine on
RewriteBase /
RewriteRule ^$ nuxt4temp/php/ [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.+)$ nuxt4temp/php/$1 [L]
</IfModule>
# 外部からの API へのアクセスを許可
Header append Access-Control-Allow-Origin: "*"
```

### 6. MySQL のセットアップ

#### `database.sql` をインポートする

phpMyAdmin を使える環境なら、データベース直下にインポートして完了です。

#### ※インポートでエラーが出たら

`database_VIEW.sql` の中身をコピーして phpMyAdmin で直接実行してください。

### 7. デフォルト API トークンの発行

このプログラムは独自のアクセストークンを利用して API にアクセスします。初回のみ以下の手順が必要です。

1. セットアップした API サーバーの `/makeApiForAdmin.php` にアクセス
2. 初回アクセス時のみ MySQL に登録処理が行われるので、表示された内容をコピー
3. `.env` に内容を記述（書き方は上記参照）
4. 以後、その値を使って API を操作できます

> **注意**: トークンを紛失した場合はリセットするしかありません（一部データは暗号化されており管理者でも確認不可）

#### デフォルト API トークンのリセット方法

1. MySQL の `api_list` テーブルの `secretId='default'` を削除
2. `api_listForView` の `secretId='default'` も同様に削除
3. 初回登録と同じ手順で再発行する

## 開発サーバーの起動

```shell
yarn dev
```

開発サーバーが `http://localhost:12345` で起動します。

## ビルド（本番用）

```shell
yarn build
```

## プレビュー（ビルド結果の確認）

```shell
yarn preview
```

## 設定のカスタマイズ

| 項目           | 設定箇所                            |
| -------------- | ----------------------------------- |
| アプリ名       | `/package.json`                     |
| フォント       | `/app/layouts/default.vue`          |
| ナビゲーション | `/app/items/itemNavigationList.js`  |
| 404 ページ     | `/app/error.vue`                    |
| Nuxt 設定      | `/nuxt.config.ts`                   |

詳しい Nuxt の設定方法は [Nuxt ドキュメント](https://nuxt.com/docs/getting-started/introduction) を参照してください。

## トラブルシューティング

### PHP がおかしい

Composer が正しくインストールされているか確認してください。

```shell
composer install
```

### yarn install でエラーが出る

Node.js のバージョンを確認してください（LTS 推奨）。

## 参考資料

- WebPush 実装参考: <https://tech.excite.co.jp/entry/2021/06/30/104213>
- Nuxt ドキュメント: <https://nuxt.com/docs/getting-started/introduction>
- Vuetify ドキュメント: <https://vuetifyjs.com/>
