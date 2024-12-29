# Eコマースプロジェクト

## プロジェクト概要

このプロジェクトは、Next.jsとSupabaseをベースにした実験的なEコマースサイトです。

## 技術スタック

### フロントエンド

- **言語:** TypeScript
- **フレームワーク:** Next.js
- **UIコンポーネント:** Chakra UI
- **スタイリング:** styled-components
- **フォーム:** react-hook-form
- **状態管理:** Zustand
- **データフェッチング:** SWR

### バックエンド

- **言語:** TypeScript
- **フレームワーク:** Node.js/Express
- **APIドキュメンテーション:** OpenAPI (Swagger)
- **データベース:** PostgreSQL (Supabase)
- **ORM:** Prisma
- **認証:** Supabase Auth

### テスト

- **フレームワーク:** Jest, React Testing Library, Vitest
- **E2Eテスト:** Cypress (オプション)

### CI/CD

- **GitHub Actions**

### インフラ

- **コンテナオーケストレーション:** Docker Swarm (軽量)
- **サーバーレス:** AWS Lambda (必要に応じて)
- **IaC:** Terraform (オプション)

### 開発ツール

- **APIクライアント:** Insomnia
- **ブラウザ開発者ツール:** Chrome DevTools
- **開発環境:** GitHub Codespaces, Docker in Docker

## ディレクトリ構造

```
ecommerce-project/
├── .devcontainer/
│   ├── devcontainer.json   # 開発コンテナの設定ファイル
│   └── Dockerfile          # 開発環境用の Dockerfile
├── app/                    # Next.js アプリケーション (フロントエンド)
│   ├── components/
│   │   ├── common/
│   │   ├── layouts/
│   │   ├── products/
│   │   └── ...
│   ├── pages/
│   │   ├── api/            # Next.js API Routes (簡易バックエンド)
│   │   ├── auth/
│   │   ├── products/
│   │   ├── _app.tsx
│   │   ├── _document.tsx
│   │   └── index.tsx
│   ├── public/
│   ├── styles/
│   ├── lib/
│   │   ├── hooks/
│   │   ├── utils/
│   │   ├── supabase.ts
│   │   └── prisma.ts
│   ├── .eslintrc.js
│   ├── .prettierrc.js
│   ├── next-env.d.ts
│   ├── next.config.js
│   ├── package.json
│   ├── tsconfig.json
│   └── Dockerfile          # 本番環境用の Dockerfile (必要に応じて)
├── backend/                # バックエンドアプリケーション (Node.js/Express)
│   ├── src/
│   │   ├── controllers/    # API コントローラー
│   │   ├── routes/         # API ルーティング
│   │   ├── services/       # ビジネスロジック
│   │   ├── types/          # 型定義
│   │   ├── middlewares/    # ミドルウェア
│   │   ├── app.ts          # Express アプリケーションのエントリーポイント
│   │   ├── server.ts       # サーバーの起動
│   │   └── index.ts
│   ├── .env.example        # 環境変数のサンプル
│   ├── .eslintrc.js
│   ├── .prettierrc.js
│   ├── package.json
│   ├── tsconfig.json
│   ├── openapi.yaml        # OpenAPI 定義ファイル
│   └── Dockerfile          # 本番環境用の Dockerfile (必要に応じて)
├── prisma/
│   ├── schema.prisma
│   └── migrations/
├── .github/                # GitHub 関連ファイル
│   └── workflows/          # GitHub Actions ワークフロー
│       └── main.yml        # CI/CD パイプライン定義
├── infra/                  # インフラ構成 (Terraform など、必要に応じて)
│   └── main.tf             # Terraform の設定ファイルなど
├── .git/
├── .gitignore
├── docker-compose.yml       # 開発環境用
├── docker-compose.prod.yml  # 本番環境用 (Docker Swarm 用、必要に応じて)
├── package.json
├── yarn.lock
├── README.md
└── .env.example            # 環境変数のサンプル
```

## 各ファイルの説明

### `.devcontainer/devcontainer.json`

開発コンテナの設定ファイル。使用するDockerfileやVS Codeの設定、拡張機能などを定義します。

### `.devcontainer/Dockerfile`

開発環境を構築するためのDockerfile。Node.jsやGit、Dockerなどの必要なツールをインストールします。

### `app/`

Next.jsアプリケーションのコードが含まれるディレクトリです。

### `backend/`

Node.js/Expressで構築されたバックエンドアプリケーションのコードが含まれるディレクトリです。

### `prisma/schema.prisma`

Prismaのスキーマ定義ファイル。データベースの構造を定義します。

### `.github/workflows/main.yml`

GitHub Actionsのワークフロー定義ファイル。CI/CDパイプラインを構築します。

### `docker-compose.yml`

開発環境で使用するDocker Composeの設定ファイル。フロントエンド、バックエンド、データベースなどのサービスを定義します。

## 開発環境の構築

1. GitHub Codespacesで新しいCodespaceを作成します。
2. VS CodeでCodespaceを開き、`.env`ファイルを作成して必要な環境変数を設定します。
3. ターミナルで`docker compose up -d`を実行し、開発環境を起動します。

## 開発フロー

### フロントエンド

`app`ディレクトリで作業します。`yarn dev`で開発サーバーを起動し、http://localhost:3000 で動作確認します。

### バックエンド

`backend`ディレクトリで作業します。`yarn start:dev`で開発サーバーを起動し、http://localhost:8080 でAPIにアクセスできます。APIドキュメントは http://localhost:8080/api-docs で確認できます。

### データベース

`prisma/schema.prisma`を編集してスキーマを定義し、`npx prisma migrate dev`でマイグレーションを実行します。`npx prisma studio`でデータベースの内容を確認できます。

## テスト

- **フロントエンド:** `yarn test` (Jest, React Testing Library)
- **バックエンド:** `yarn test` (Jest)

## Git コミット規約

- **絵文字:**  コミットの先頭に絵文字を付与します (例: ✨ 機能追加, 🐛 バグ修正)。
- **メッセージ:** 日本語でタイトルと概要を記述します。

## その他の情報

- **package.json:** プロジェクトの依存関係やスクリプトが定義されています。
- **yarn.lock:** 依存関係のバージョンを固定します。
- **.gitignore:** Gitの管理対象から除外するファイルを指定します。
