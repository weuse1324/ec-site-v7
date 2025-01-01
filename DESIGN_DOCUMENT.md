# 設計ドキュメント

このドキュメントは、このプロジェクトの設計について記述します。

## 概要

このプロジェクトは、Next.js と Supabase をベースにした実験的な E コマースサイトです。

## アーキテクチャ

システムの全体構成は以下の通りです。

- **フロントエンド:** Next.js で構築された Web アプリケーション
- **バックエンド:** Node.js/Express で構築された API サーバー
- **データベース:** Supabase (PostgreSQL)

フロントエンドとバックエンドは API 経由で通信を行います。

## データモデル

主なデータモデルは以下の通りです。

- **Product:** 商品情報 (ID, 名前, 説明, 価格, 画像など)

## API設計

(現時点では詳細な API 設計は未定)

## 技術スタック

- **フロントエンド:** TypeScript, Next.js, Chakra UI, styled-components, react-hook-form, Zustand, SWR
- **バックエンド:** TypeScript, Node.js/Express, Prisma
- **データベース:** PostgreSQL (Supabase)

## セキュリティ

(現時点では具体的なセキュリティ対策は未定)

## デプロイメント

(現時点では具体的なデプロイメント戦略は未定)
