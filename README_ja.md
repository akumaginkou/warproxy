# WARProxy

Cloudflare WARPを1つのコマンドでSOCKS5/HTTPプロキシサーバーに変換！
必要な機能をすべて含んだ最小限のDockerイメージ！

含まれる機能:
- WGCF: Cloudflare WARPアカウントとWireGuard設定を自動生成
- WireProxy: SOCKS5プロキシを作成
- Warp+: WARP+通信量を自動更新
- ヘルスチェックなど

## 使い方

`docker`:

```sh
docker run --name warproxy \
  -p 1080:1080 \
  -d ghcr.io/kingcc/warproxy:latest
```

または `docker-compose`:

```yaml
services:
  warproxy:
    image: ghcr.io/akumaginkou/warproxy:latest
    restart: always
    ports:
      - 1080:1080
```


## 環境変数

| 環境変数  | 説明  | デフォルト値  |
|---|---|---|
| ```PUID``` / ```PGID```  | アプリケーション実行のためのuid/gid  | ```911``` / ```911```  |
| ```TZ```  | タイムゾーン  | ```Asia/Shanghai```  |
| ```SOCKS5_PORT```  | SOCKS5プロキシのポート番号  | ```1080``` |
| ```USERNAME```  | SOCKS5認証のユーザー名  | なし |
| ```PASSWORD```  | SOCKS5認証のパスワード  | なし |
| ```HTTP_PORT```  | HTTPプロキシのポート番号  | なし |
| ```ENDPOINT```  | Cloudflareのエンドポイント | ```engage.cloudflareclient.com``` |
| ```DNS```  | リモートDNSオプション  | ```1.1.1.1``` |
| ```LICENSE_KEY```  | WARP+ライセンスキー  | なし |
| ```WARP_PLUS```  | ```true```に設定するとWARP+自動更新スクリプトを有効化  | ```false``` |
| ```VERBOSE```  | 詳細ログを表示   | ```false```  |

### WARP+ライセンスキーの使用方法
既存のWARP+ライセンスキーをお持ちの場合、`LICENSE_KEY`環境変数で設定できます:

```yaml
services:
  warproxy:
    image: ghcr.io/akumaginkou/warproxy:latest
    restart: always
    environment:
      - LICENSE_KEY=your-license-key-here
    ports:
      - 1080:1080
```

alternatively、手動で `/config/wgcf-account.toml` を編集し、2つのファイルを削除することもできます:
`/config/wgcf-profile.conf` と `/config/wireproxy.conf`
コンテナを再起動すると、アカウント情報が更新され、設定ファイルが自動的に再生成されます。


## Thanks
* [105PM](https://github.com/105PM/docker-warproxy)
* [by275 (docker-dpitunnel)](https://github.com/by275/docker-dpitunnel)
