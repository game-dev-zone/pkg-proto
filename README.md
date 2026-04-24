# pkg-proto

Club No.8 全站共用的 Protobuf / gRPC 契約模組。以 `buf` 管理，git tag 發版（`v1.x.y`）。

## 目錄

```
proto/
├── common/v1/common.proto        # UserInfo / Wallet / ErrorCode / Ack
├── game/v1/game_service.proto    # GameService (CreateRoom/Enter/Bet/Settle/Subscribe)
├── tx/v1/tx_service.proto        # TxService (Transfer/GetBalance/QueryRecords)
└── gw/v1/envelope.proto          # Envelope（WebSocket 外層封包）
gen/go/                           # buf generate 產出（gitignore）
```

## 常用指令

```bash
buf lint                          # 檢查規範
buf breaking --against '.git#branch=main'
buf generate                      # 產出 gen/go/
go build ./...                    # 驗證產物
```

## Go Module

```
module github.com/club8/pkg-proto
```

其他服務 import 範例：

```go
import (
    commonv1 "github.com/club8/pkg-proto/gen/go/common/v1"
    gamev1   "github.com/club8/pkg-proto/gen/go/game/v1"
)
```
