# 本番デプロイ手順

## 前提条件
- VPSにDocker & Docker Compose v2がインストール済み
- ドメイン `pma.newtralize.com` のAレコードがVPSのIPに設定済み
- Traefikが起動済みで `traefik` ネットワークが存在

## デプロイ手順

### 1. コードの取得
```bash
git clone https://github.com/yasuhiro112358/private-db-stack.git
cd private-db-stack
```

### 2. 本番環境変数の設定
```bash
# 強力なパスワードを生成
openssl rand -base64 32

# .envファイルを作成（本番用）
echo "MYSQL_ROOT_PASSWORD=生成したパスワード" > .env

# ファイル権限を制限
chmod 600 .env
```

### 3. デプロイ実行
```bash
# 本番構成で起動
docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```

### 4. 動作確認
```bash
# コンテナ状態確認
docker compose -f docker-compose.yml -f docker-compose.prod.yml ps

# ログ確認
docker compose -f docker-compose.yml -f docker-compose.prod.yml logs

# HTTPSアクセステスト
curl -I https://pma.newtralize.com
```

## セキュリティチェック
- [ ] .env.prodがGitに含まれていない
- [ ] MySQLポートが外部公開されていない
- [ ] HTTPS証明書が正しく取得されている
- [ ] Basic認証などの追加保護を検討

## 更新手順
```bash
# コード更新
git pull origin main

# 再デプロイ
docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```

## トラブルシューティング
```bash
# コンテナログ確認
docker compose -f docker-compose.yml -f docker-compose.prod.yml logs mysql
docker compose -f docker-compose.yml -f docker-compose.prod.yml logs phpmyadmin

# ネットワーク確認
docker network ls | grep traefik

# Traefik確認
docker compose -f /path/to/traefik/docker-compose.yml logs
```
