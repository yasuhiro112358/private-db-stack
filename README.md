# Private DB Stack

MySQL + phpMyAdmin を Docker Compose で構築する私用DB環境。  
外部公開は Traefik 経由（TLS）で行い、MySQL は内部ネットワークのみで運用します。

## 前提
- Docker / Docker Compose が導入済み
- 逆プロキシ（Traefik）スタックが稼働し、`proxy` ネットワークが存在
- このスタック用に内部ネットワーク `backend` を共用する

## セットアップ
```bash
git clone <YOUR-REPO-URL> private-db-stack
cd private-db-stack
cp .env.example .env
# .env を編集（パスワード、ドメインなど）
docker network create proxy   # 未作成なら
docker network create backend # 未作成なら
docker compose up -d
