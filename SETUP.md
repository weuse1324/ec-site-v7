# 開発環境構築手順

このドキュメントでは、このプロジェクトの開発環境を構築するための手順について説明します。

## 前提条件

- Node.js (推奨バージョン: 16.x 以上)
- npm または yarn
- Git

## 手順

1. リポジトリをクローンします。
   ```bash
   git clone [リポジトリのURL]
   cd ec-site-v7
   ```

2. フロントエンドの依存関係をインストールします。
   ```bash
   cd app
   npm install
   ```
   または
   ```bash
   yarn install
   ```

3. バックエンドの依存関係をインストールします。
   ```bash
   cd ../backend
   npm install
   ```
   または
   ```bash
   yarn install
   ```

4. 環境変数を設定します。
   - プロジェクトルートディレクトリにある `.env.example` ファイルをコピーして `.env` ファイルを作成します。
   - `.env` ファイルに必要な環境変数の値を設定します。

5. データベースを設定します。
   - Supabase プロジェクトを作成し、データベースの接続情報を取得します。
   - `prisma/schema.prisma` ファイルを編集し、Supabase のデータベース接続情報を設定します。
   - Prisma Migrate を実行して、データベースのスキーマを適用します。
     ```bash
     cd ../prisma
     npx prisma migrate dev
     ```

6. 開発サーバーを起動します。
   - フロントエンドの開発サーバーを起動します。
     ```bash
     cd ../app
     npm run dev
     ```
     または
     ```bash
     yarn dev
     ```
   - バックエンドの開発サーバーを起動します。
     ```bash
     cd ../backend
     npm run dev
     ```
     または
     ```bash
     yarn dev
     ```

これで開発環境の構築は完了です。
