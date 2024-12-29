2.0 Experimental Advanced. Lacks access to real-time info and some Gemini features.
承知しました。ご指定の要件を追加し、さらにブラッシュアップしたバージョンを提示します。ディレクトリ構造、各ファイルの内容、package.json、推奨ライブラリ、そして開発・運用フローまで、詳細に説明します。

プロジェクト要件 (更新):

フロントエンド:
言語: TypeScript
フレームワーク: Next.js
UIコンポーネント: Chakra UI
スタイリング: styled-components
フォーム: react-hook-form
状態管理: Zustand
データフェッチング: SWR
バックエンド:
言語: TypeScript
フレームワーク: Node.js/Express
APIドキュメンテーション: OpenAPI (Swagger)
データベース: PostgreSQL (Supabase)
ORM: Prisma
認証: Supabase Auth
テスト:
フレームワーク: Jest, React Testing Library, Vitest (ブラウザを使用しない、高速なテスティングフレームワーク)
E2Eテスト: Cypress (オプション)
CI/CD: GitHub Actions
コンテナオーケストレーション: Docker Swarm (軽量なコンテナオーケストレーション)
サーバーレス: AWS Lambda (必要に応じて)
IaC: Terraform (オプション)
APIクライアント: Insomnia
ブラウザ開発者ツール: Chrome DevTools
その他:
GitHub Codespaces で Docker in Docker を使用
.devcontainer で開発環境を構築
docker compose up で開発環境を起動
ECサイト特有ツール/サービス:
決済代行サービス: Stripe, PayPal
メール配信サービス: SendGrid, Mailchimp
CRM: Salesforce, HubSpot (必要に応じて)
コミュニケーションツール: Slack
AI: OpenAI API (必要に応じて)
ディレクトリ構造:

ecommerce-project/
├── .devcontainer/
│   ├── devcontainer.json   # 開発コンテナの設定ファイル
│   └── Dockerfile          # 開発環境用の Dockerfile
├── app/                    # Next.js アプリケーション (フロントエンド)
│   ├── components/
│   │   ├── common/
│   │   │   ├── Button.tsx
│   │   │   ├── Input.tsx
│   │   │   └── ...
│   │   ├── layouts/
│   │   │   └── MainLayout.tsx
│   │   ├── products/
│   │   │   ├── ProductCard.tsx
│   │   │   ├── ProductList.tsx
│   │   │   └── ...
│   │   └── ...
│   ├── pages/
│   │   ├── api/            # Next.js API Routes (簡易バックエンド)
│   │   │   ├── products/
│   │   │   │   ├── [id].ts
│   │   │   │   └── index.ts
│   │   │   └── ...
│   │   ├── auth/
│   │   │   ├── login.tsx
│   │   │   ├── signup.tsx
│   │   │   └── ...
│   │   ├── products/
│   │   │   ├── [id].tsx
│   │   │   └── index.tsx
│   │   ├── _app.tsx
│   │   ├── _document.tsx
│   │   └── index.tsx
│   ├── public/
│   │   └── favicon.ico
│   ├── styles/
│   │   └── globals.css
│   ├── lib/
│   │   ├── hooks/
│   │   │   └── useProducts.ts
│   │   ├── utils/
│   │   │   └── index.ts
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
│   │   │   └── productController.ts
│   │   ├── routes/         # API ルーティング
│   │   │   └── productRoutes.ts
│   │   ├── services/       # ビジネスロジック
│   │   │   └── productService.ts
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
各ファイルの内容 (抜粋):

1. .devcontainer/devcontainer.json:

JSON

{
  "name": "Next.js & Supabase E-commerce Dev Container",
  "dockerFile": "./Dockerfile",
  "forwardPorts": [3000, 8080], // 3000 for frontend, 8080 for backend
  "remoteUser": "node",
  "runArgs": ["--init"],
  "mounts": [
    "source=/var/run/docker.sock,target=/var/run/docker.sock,type=bind"
  ],
  "overrideCommand": false,
  "remoteEnv": {
    "LOCAL_WORKSPACE_FOLDER": "${localWorkspaceFolder}",
    "DATABASE_URL": "postgresql://user:password@db:5432/ecommerce_db",
    "NEXT_PUBLIC_SUPABASE_URL": "${localWorkspaceFolder}/.env",
    "NEXT_PUBLIC_SUPABASE_ANON_KEY": "${localWorkspaceFolder}/.env",
    "BACKEND_PORT": "8080"
  },
  "customizations": {
    "vscode": {
      "settings": {
        "editor.formatOnSave": true,
        "editor.defaultFormatter": "esbenp.prettier-vscode",
        "[javascript]": {
          "editor.defaultFormatter": "esbenp.prettier-vscode"
        },
        "[typescript]": {
          "editor.defaultFormatter": "esbenp.prettier-vscode"
        },
        "[typescriptreact]": {
          "editor.defaultFormatter": "esbenp.prettier-vscode"
        },
        "editor.codeActionsOnSave": {
          "source.fixAll.eslint": true
        },
        "editor.tabSize": 2,
        "files.eol": "\n",
        "typescript.tsdk": "node_modules/typescript/lib",
        "terminal.integrated.profiles.linux": {
          "bash": {
            "path": "/bin/bash"
          }
        },
        "terminal.integrated.defaultProfile.linux": "bash"
      },
      "extensions": [
        "dbaeumer.vscode-eslint",
        "esbenp.prettier-vscode",
        "eamodio.gitlens",
        "formulahendry.auto-rename-tag",
        "streetsidesoftware.code-spell-checker",
        "yzhang.markdown-all-in-one",
        "mhutchie.git-graph",
        "Prisma.prisma",
        "ZixuanChen.vitest-explorer",
        "redhat.vscode-yaml",
        "ms-vscode.vscode-typescript-next"
      ]
    }
  },
  "postCreateCommand": "yarn install && npx prisma generate",
  "postStartCommand": "bash .devcontainer/postStartCommand.sh"
}
2. .devcontainer/Dockerfile:

Dockerfile

FROM node:lts-bullseye

RUN apt-get update && apt-get install -y \
    git \
    curl \
    vim \
    htop \
    procps \
    openssl \
    libpq-dev \
    postgresql-client

RUN curl -fsSL https://get.docker.com | sh

# Install Docker Compose (if not using docker compose CLI in docker engine)
RUN curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
RUN chmod +x /usr/local/bin/docker-compose

EXPOSE 3000
EXPOSE 8080

USER node

ARG NONROOT_USER=node
RUN echo "#!/bin/sh\n\
    sudoIf() { if [ \"\$(id -u)\" -ne 0 ]; then sudo \"\$@\"; else \"\$@\"; fi }\n\
    SOCKET_GID=\$(stat -c '%g' /var/run/docker.sock) \n\
    if [ \"${SOCKET_GID}\" != '0' ]; then\n\
        if [ \"\$(cat /etc/group | grep :\${SOCKET_GID}:)\" = '' ]; then sudoIf groupadd --gid \${SOCKET_GID} docker-host; fi \n\
        if [ \"\$(id ${NONROOT_USER} | grep -E \"groups=.*(=|,)\${SOCKET_GID}\(\")\" = '' ]; then sudoIf usermod -aG \${SOCKET_GID} ${NONROOT_USER}; fi\n\
    fi\n\
    exec \"\$@\"" > /usr/local/share/docker-init.sh \
    && chmod +x /usr/local/share/docker-init.sh

ENTRYPOINT [ "/usr/local/share/docker-init.sh" ]
CMD [ "sleep", "infinity" ]
3. .devcontainer/postStartCommand.sh

Bash

#!/bin/bash
npx prisma migrate dev --name init
4. app/Dockerfile (本番環境用 - 必要に応じて):

Dockerfile

# 本番環境用のイメージはスリムなものを使う
FROM node:lts-bullseye-slim

WORKDIR /app

COPY package*.json ./

COPY yarn.lock ./

RUN yarn install --frozen-lockfile --production

COPY . .

EXPOSE 3000

CMD ["yarn", "start"]
5. backend/Dockerfile (本番環境用 - 必要に応じて):

Dockerfile

# 本番環境用のイメージはスリムなものを使う
FROM node:lts-bullseye-slim

WORKDIR /app

COPY package*.json ./

COPY yarn.lock ./

RUN yarn install --frozen-lockfile --production

COPY . .

EXPOSE 8080

CMD ["yarn", "start"]
6. docker-compose.yml:

YAML

version: "3.9"
services:
  app:
    build:
      context: app
      dockerfile: Dockerfile
    volumes:
      - type: bind
        source: ${LOCAL_WORKSPACE_FOLDER:-.}/app
        target: /app
    command: yarn dev
    ports:
      - 3000:3000
    environment:
      - DATABASE_URL=postgresql://user:password@db:5432/ecommerce_db
      - NEXT_PUBLIC_SUPABASE_URL=${NEXT_PUBLIC_SUPABASE_URL}
      - NEXT_PUBLIC_SUPABASE_ANON_KEY=${NEXT_PUBLIC_SUPABASE_ANON_KEY}
      - BACKEND_URL=http://backend:8080
    networks:
      - myapp
    depends_on:
      - db
      - backend
  backend:
    build:
      context: backend
      dockerfile: Dockerfile
    volumes:
      - type: bind
        source: ${LOCAL_WORKSPACE_FOLDER:-.}/backend
        target: /app
    command: yarn start:dev
    ports:
      - 8080:8080
    environment:
      - DATABASE_URL=postgresql://user:password@db:5432/ecommerce_db
      - PORT=8080
    networks:
      - myapp
    depends_on:
      - db
  db:
    image: postgres:14
    restart: always
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: ecommerce_db
    ports:
      - 5432:5432
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - myapp

volumes:
  postgres_data:

networks:
  myapp:
    name: myapp
7. docker-compose.prod.yml (本番環境用 - Docker Swarm 用、必要に応じて):

YAML

version: "3.9"
services:
  app:
    image: your-docker-registry/app:latest # ビルドしたイメージを指定
    deploy:
      replicas: 3 # 3つのレプリカを作成
      placement:
        constraints:
          - node.role == worker
      update_config:
        parallelism: 1
        delay: 10s
      restart_policy:
        condition: on-failure
    ports:
      - 3000:3000
    environment:
      - DATABASE_URL=postgresql://user:password@db:5432/ecommerce_db # 適切な値に変更
      - NEXT_PUBLIC_SUPABASE_URL=${NEXT_PUBLIC_SUPABASE_URL} # 適切な値に変更
      - NEXT_PUBLIC_SUPABASE_ANON_KEY=${NEXT_PUBLIC_SUPABASE_ANON_KEY} # 適切な値に変更
      - BACKEND_URL=http://backend:8080
    networks:
      - myapp
    depends_on:
      - db
      - backend

  backend:
    image: your-docker-registry/backend:latest # ビルドしたイメージを指定
    deploy:
      replicas: 2
      placement:
        constraints:
          - node.role == worker
      update_config:
        parallelism: 1
        delay: 10s
      restart_policy:
        condition: on-failure
    ports:
      - 8080:8080
    environment:
      - DATABASE_URL=postgresql://user:password@db:5432/ecommerce_db # 適切な値に変更
      - PORT=8080
    networks:
      - myapp
    depends_on:
      - db

  db:
    image: postgres:14
    deploy:
      placement:
        constraints:
          - node.role == manager # マネージャーノードに配置
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: ecommerce_db
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - myapp

volumes:
  postgres_data:

networks:
  myapp:
    name: myapp
    driver: overlay
8. app/lib/supabase.ts:

TypeScript

import { createClient } from '@supabase/supabase-js';

const supabaseUrl = process.env.NEXT_PUBLIC_SUPABASE_URL || '';
const supabaseAnonKey = process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY || '';

export const supabase = createClient(supabaseUrl, supabaseAnonKey);
9. app/lib/prisma.ts:

TypeScript

import { PrismaClient } from '@prisma/client';

const globalForPrisma = global as unknown as { prisma: PrismaClient };

export const prisma =
  globalForPrisma.prisma ||
  new PrismaClient({
    log: ['query'],
  });

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma;
10. prisma/schema.prisma:

Code snippet

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

model Product {
  id          Int     @id @default(autoincrement())
  name        String
  description String?
  price       Int
  image       String?
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt





続きは




2.0 Experimental Advanced. Lacks access to real-time info and some Gemini features.
続きを説明します。前回の説明で、prisma/schema.prisma まで完了しましたので、package.json、app/package.json、backend 関連のファイル、GitHub Actions、OpenAPI、そして開発・運用フローについて説明します。

11. package.json (プロジェクトルート):

JSON

{
  "name": "ecommerce-project",
  "version": "1.0.0",
  "private": true,
  "workspaces": [
    "app",
    "backend"
  ],
  "scripts": {
    "dev": "docker compose up -d",
    "stop": "docker compose down",
    "prisma:generate": "prisma generate",
    "prisma:migrate:dev": "prisma migrate dev",
    "prisma:migrate:deploy": "prisma migrate deploy",
    "prisma:studio": "prisma studio",
    "test": "yarn workspace @my-ecommerce/app test",
    "test:backend": "yarn workspace @my-ecommerce/backend test",
    "build": "yarn workspace @my-ecommerce/app build",
    "postinstall": "husky install"
  },
  "dependencies": {
    "@prisma/client": "^4.0.0"
  },
  "devDependencies": {
    "@testing-library/jest-dom": "^5.16.5",
    "@testing-library/react": "^13.4.0",
    "@testing-library/user-event": "^14.4.3",
    "cypress": "^12.5.1",
    "husky": "^8.0.3",
    "jest": "^29.3.1",
    "jest-environment-jsdom": "^29.3.1",
    "prisma": "^4.0.0"
  }
}
12. app/package.json:

JSON

{
  "name": "@my-ecommerce/app",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev -H 0.0.0.0",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "test": "jest",
    "test:watch": "jest --watch",
    "test:cov": "jest --coverage",
    "cypress:open": "cypress open",
    "cypress:run": "cypress run"
  },
  "dependencies": {
    "@chakra-ui/react": "^2.4.9",
    "@emotion/react": "^11.10.5",
    "@emotion/styled": "^11.10.5",
    "@supabase/supabase-js": "^2.7.1",
    "framer-motion": "^9.0.2",
    "next": "latest",
    "react": "latest",
    "react-dom": "latest",
    "react-hook-form": "^7.43.1",
    "styled-components": "^5.3.6",
    "swr": "^2.0.3",
    "zustand": "^4.3.2"
  },
  "devDependencies": {
    "@testing-library/react": "^13.4.0",
    "@types/jest": "^29.2.6",
    "@types/node": "latest",
    "@types/react": "latest",
    "@types/react-dom": "latest",
    "@types/styled-components": "^5.1.26",
    "@vitejs/plugin-react": "^3.0.1",
    "eslint": "latest",
    "eslint-config-next": "latest",
    "typescript": "latest",
    "vitest": "^0.28.3"
  }
}
13. backend/package.json:

JSON

{
  "name": "@my-ecommerce/backend",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "build": "tsc",
    "start": "node dist/index.js",
    "start:dev": "ts-node-dev --respawn --transpile-only src/index.ts",
    "test": "jest",
    "test:watch": "jest --watch",
    "test:cov": "jest --coverage",
    "lint": "eslint . --ext .ts",
    "format": "prettier --write \"**/*.{ts,tsx,js,jsx,json}\"",
    "postinstall": "husky install"
  },
  "dependencies": {
    "@prisma/client": "^4.0.0",
    "cors": "^2.8.5",
    "dotenv": "^16.0.3",
    "express": "^4.18.2",
    "express-async-errors": "^3.1.1",
    "helmet": "^6.0.1",
    "http-status-codes": "^2.2.0",
    "morgan": "^1.10.0",
    "swagger-ui-express": "^4.6.0",
    "winston": "^3.8.2",
    "zod": "^3.20.2"
  },
  "devDependencies": {
    "@types/cors": "^2.8.13",
    "@types/express": "^4.17.16",
    "@types/jest": "^29.2.6",
    "@types/morgan": "^1.9.4",
    "@types/node": "^18.11.18",
    "@types/supertest": "^2.0.12",
    "@types/swagger-ui-express": "^4.1.3",
    "@typescript-eslint/eslint-plugin": "^5.48.2",
    "@typescript-eslint/parser": "^5.48.2",
    "eslint": "^8.32.0",
    "eslint-config-airbnb-base": "^15.0.0",
    "eslint-config-prettier": "^8.6.0",
    "eslint-plugin-import": "^2.27.5",
    "eslint-plugin-jest": "^27.2.1",
    "eslint-plugin-prettier": "^4.2.1",
    "husky": "^8.0.3",
    "jest": "^29.3.1",
    "lint-staged": "^13.1.0",
    "prettier": "^2.8.3",
    "supertest": "^6.3.3",
    "ts-jest": "^29.0.5",
    "ts-node-dev": "^2.0.0",
    "typescript": "^4.9.4"
  }
}
14. backend/src/index.ts (エントリーポイント):

TypeScript

import app from './app';
import { logger } from './utils/logger';

const port = process.env.PORT || 8080;

app.listen(port, () => {
  logger.info(`Server is running on port ${port}`);
});
15. backend/src/app.ts (Express アプリケーション):

TypeScript

import express from 'express';
import cors from 'cors';
import helmet from 'helmet';
import morgan from 'morgan';
import { productRouter } from './routes/productRoutes';
import swaggerUi from 'swagger-ui-express';
import YAML from 'yamljs';
import path from 'path';
import 'express-async-errors'

const app = express();

// Middleware
app.use(cors());
app.use(helmet());
app.use(morgan('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// API Routes
app.use('/api/products', productRouter);

// OpenAPI (Swagger) Documentation
const swaggerDocument = YAML.load(path.join(__dirname, '../openapi.yaml'));
app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerDocument));

// Error handling middleware (must be defined after all other routes)

export default app;
16. backend/src/controllers/productController.ts:

TypeScript

import { Request, Response } from 'express';
import { StatusCodes } from 'http-status-codes';
import { productService } from '../services/productService';

export const productController = {
  getAllProducts: async (req: Request, res: Response) => {
    const products = await productService.getAllProducts();
    res.status(StatusCodes.OK).json(products);
  },

  getProductById: async (req: Request, res: Response) => {
    const { id } = req.params;
    const product = await productService.getProductById(Number(id));
    if (!product) {
      return res.status(StatusCodes.NOT_FOUND).json({ message: 'Product not found' });
    }
    res.status(StatusCodes.OK).json(product);
  },

  createProduct: async (req: Request, res: Response) => {
    // ... (バリデーションなどの処理を追加)
    const newProduct = await productService.createProduct(req.body);
    res.status(StatusCodes.CREATED).json(newProduct);
  },

  // ... 他の CRUD 操作のメソッドを追加 (update, delete など)
};
17. backend/src/services/productService.ts:

TypeScript

import { PrismaClient, Product } from '@prisma/client';

const prisma = new PrismaClient();

export const productService = {
  getAllProducts: async (): Promise<Product[]> => {
    return prisma.product.findMany();
  },

  getProductById: async (id: number): Promise<Product | null> => {
    return prisma.product.findUnique({ where: { id } });
  },

  createProduct: async (data: Omit<Product, 'id' | 'createdAt' | 'updatedAt'>): Promise<Product> => {
    return prisma.product.create({ data });
  },

  // ... 他の CRUD 操作のメソッドを追加 (update, delete など)
};
18. backend/src/routes/productRoutes.ts:

TypeScript

import { Router } from 'express';
import { productController } from '../controllers/productController';

const router = Router();

router.get('/', productController.getAllProducts);
router.get('/:id', productController.getProductById);
router.post('/', productController.createProduct);
// ... 他のルートを追加 (update, delete など)

export { router as productRouter };
19. backend/src/types/index.ts (必要に応じて型定義):

TypeScript

// 例:
export interface Product {
  id: number;
  name: string;
  description?: string;
  price: number;
  imageUrl?: string;
}
20. backend/openapi.yaml:

YAML

openapi: 3.0.0
info:
  title: E-commerce API
  version: 1.0.0
paths:
  /api/products:
    get:
      summary: Get all products
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Product'
    post:
      summary: Create a new product
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/ProductInput'
      responses:
        '201':
          description: Product created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Product'
  /api/products/{id}:
    get:
      summary: Get a product by ID
      parameters:
        - in: path
          name: id
          required: true
          schema:
            type: integer
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Product'
        '404':
          description: Product not found
components:
  schemas:
    Product:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        description:
          type: string
        price:
          type: integer
        image:
          type: string
        createdAt:
          type: string
          format: date-time
        updatedAt:
          type: string
          format: date-time
    ProductInput:
      type: object
      required:
        - name
        - price
      properties:
        name:
          type: string
        description:
          type: string
        price:
          type: integer
        image:
          type: string
21. .github/workflows/main.yml (GitHub Actions):

YAML

name: CI/CD

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 'lts/*'
      - name: Install dependencies
        run: yarn install --frozen-lockfile
      - name: Run tests (Frontend)
        run: yarn test:cov
        working-directory: app
      - name: Run tests (Backend)
        run: yarn test:cov
        working-directory: backend

  # 以下のジョブは必要に応じて追加・設定

  # build:
  #   needs: test
  #   runs-on: ubuntu-latest
  #   steps:
  #     - uses: actions/checkout@v3
  #     - name: Set up Node.js
  #       uses: actions/setup-node@v3
  #       with:
  #         node-version: 'lts/*'
  #     - name: Install dependencies
  #       run: yarn install --frozen-lockfile
  #     - name: Build Frontend
  #       run: yarn build
  #       working-directory: app
  #     - name: Build Backend
  #       run: yarn build
  #       working-directory: backend
  #     - name: Build Docker images
  #       run: |
  #         docker build -t your-docker-registry/app:latest .
  #         docker build -t your-docker-registry/backend:latest .
  #       working-directory: .

  # deploy:
  #   needs: build
  #   runs-on: ubuntu-latest
  #   steps:
  #     - name: Deploy to production
  #       run: |
  #         # 部署コマンド (例: Docker Swarm へのデプロイ)
  #         docker stack deploy -c docker-compose.prod.yml ecommerce
22. backend/src/utils/logger.ts (ロガー):

TypeScript

import winston from 'winston';

export const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  defaultMeta: { service: 'backend' },
  transports: [
    new winston.transports.Console({
      format: winston.format.simple(),
    }),
  ],
});
開発・運用フロー:

開発環境構築:

GitHub Codespaces で新しい Codespace を作成します。
VS Code で Codespace を開き、.env ファイルを作成して必要な環境変数を設定します。
ターミナルで docker compose up -d を実行し、開発環境を起動します。
フロントエンド開発 (Next.js):

app ディレクトリで作業します。
yarn dev で開発サーバーを起動し、http://localhost:3000 で動作確認できます。
app/pages/api 以下にAPIエンドポイントを簡易的に作れます。
バックエンド開発 (Express):

backend ディレクトリで作業します。
yarn start:dev で開発サーバーを起動し、http://localhost:8080 でAPIにアクセスできます。
http://localhost:8080/api-docsでOpenAPI(Swagger)のドキュメントを確認できます。
データベース操作 (Prisma):

prisma/schema.prisma を編集して、データベーススキーマを定義します。
npx prisma generate で Prisma Client を生成します。
npx prisma migrate dev でデータベースにマイグレーションを適用します。
npx prisma studio で Prisma Studio を起動し、データベースの内容を確認・編集できます。
テスト:

フロントエンド: yarn test (Jest, React Testing Library), yarn test:watch, yarn test:cov, yarn cypress:open(必要に応じて)
