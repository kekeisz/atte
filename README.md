# Atte
企業向けの勤怠管理アプリです。</br>
</br>
ホーム画面
<img width="1470" alt="Atte ホーム画面" src="https://github.com/user-attachments/assets/50d9424c-0df2-4983-bc59-bcc3823b1f2a">

## 作成した目的
企業における人事評価のため

## 機能一覧
・会員登録機能</br>
・ログイン機能</br>
・ログアウト機能</br>
・勤務開始・終了時間の記録</br>
・合計勤務時間の記録</br>
・休憩開始・終了時間の記録</br>
・合計休憩時間の記録</br>
・日付別勤怠情報取得</br>
・従業員一覧表示</br>
・従業員別勤怠情報取得

## 使用技術（実行環境）
Laravel8.x

### 構築環境
・php:7.4.9-fpm
</br>
・nginx:1.21.1
</br>
・mysql:8.0.26

## テーブル設計
<img width="598" alt="Atte テーブル仕様書" src="https://github.com/user-attachments/assets/9564a033-884d-45aa-bf6a-7ae19502680e">

## ER図
<img width="718" alt="Atte ER図" src="https://github.com/user-attachments/assets/ba90d09f-5163-40fe-aa4d-9d7b45d54715">

## 環境構築

### Gitからクローン

```
$ git clone git@github.com:kekeisz/atte.git sample_app
```
`sample_app`を任意の名前にする。

### ローカルリポジトリから紐付け先を変更

```
$ cd sample_app
$ git remote set-url origin 変更先のリポジトリのurl
```

### リモートリポジトリにデータを反映

```
$ sudo chmod -R 777 *
$ git add .
$ git commit -m "任意のコミットメッセージ"
$ git push origin main
```

### Docker-Compose　イメージをビルド

`docker-composer.yml`ファイル内の`app: build`を元に`docker/php/Dockerfile`の内容を実行。
```
$docker-compose up -d --build
```

### .envファイルの作成

PHPコンテナにログイン
```
$docker-compose exec php bash
```

`.env.example`ファイルをコピーして`.env`ファイルを作成
```
PHPコンテナ内
$ cp .env.example .env
```

### Laravelのパッケージのインストール

```
PHPコンテナ上
$ composer install
```

### VSCodeを開き、.envファイルの11行目以降を編集

```
.envファイル
DB_CONNECTION=mysql
- DB_HOST=127.0.0.1
+ DB_HOST=mysql
DB_PORT=3306
- DB_DATABASE=laravel
- DB_USERNAME=root
- DB_PASSWORD=
+ DB_DATABASE=laravel_db
+ DB_USERNAME=laravel_user
+ DB_PASSWORD=laravel_pass

//中略

MAIL_MAILER=smtp
-MAIL_HOST=mailhog
-MAIL_PORT=1025
-MAIL_USERNAME=null
-MAIL_PASSWORD=null
-MAIL_ENCRYPTION=null
-MAIL_FROM_ADDRESS=null
+MAIL_HOST=smtp.gmail.com
+MAIL_PORT=587
+MAIL_USERNAME=your@gmail.com //認証メール送信元のメールアドレス
+MAIL_PASSWORD=
+MAIL_ENCRYPTION=tls
+MAIL_FROM_ADDRESS=your@gmail.com //認証メール送信元のメールアドレス
MAIL_FROM_NAME="${APP_NAME}"
```

### Google アカウントでアプリパスワードを生成

Google アカウントのセキュリティページから「2段階認証」を有効にする。（まだ有効にしていない場合）</br>
「アプリパスワード」を選択し、アプリケーションとして「メール」を選んでアプリパスワードを生成。</br>
生成されたアプリパスワードをコピーし、`.env`ファイルの MAIL_PASSWORD に貼り付ける。
```
MAIL_MAILER=smtp
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=kekeisozaki13@gmail.com
MAIL_PASSWORD=MAIL_PASSWORD=your_generated_app_password
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=kekeisozaki13@gmail.com
MAIL_FROM_NAME="${APP_NAME}"
```
Laravelのキャッシュをクリア
```
$ php artisan config:clear
$ php artisan config:cache
```

###  暗号キーの生成

```
PHPコンテナ上
$ php artisan key:generate
```

### テーブルの作成

```
PHPコンテナ上
$ php artisan migrate
```

