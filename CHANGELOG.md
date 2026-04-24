# pkg-proto Changelog

本檔以 [Keep a Changelog](https://keepachangelog.com/) 格式記錄 `github.com/club8/pkg-proto` 的公開契約變更。

版本策略：
- 以 semver 發佈 git tag（例：`v1.0.0`）。
- 向後相容的欄位新增 → 次版本號提升。
- 破壞性變更（改名、刪除、改型）→ 主版本號提升，並在此檔案醒目標註。
- `club.<domain>.v1` 一律僅做相容性演進；破壞性變更須開 `v2` 平行檔案，舊 `v1` 保留直到客戶端下線。

## [Unreleased]

### Added
- `club.record.v1`：新增 Record 域。`RecordService.WriteGameRecord` 冪等寫入一場對局審計紀錄；`RecordService.QueryGameRecords` 支援 user/game/room 過濾與 cursor 分頁。配套的 Go service 位於 `club+server/record/`。
- `pkg-game-framework` 模組（`github.com/club8/pkg-game-framework`）以此 proto 為依賴，於 Settle 成功後背景寫入紀錄。

### Changed
- **Idempotency key 前綴規則（framework 層）**：由 `pkg-game-framework` 產生的 key 一律帶 `<game_id>-` 前綴（例：`ddz-bet-<room_id>-<user_id>-<amount>`、`ddz-settle-<room_id>`）。此規則不影響 proto，但消費者若手寫 idempotency key，建議對齊。`game-ddz` 於此次遷移前的 `tx_entries` 舊紀錄屬於無前綴的舊命名空間，**不回填**。

## [v1.0.0] — 首次公開 tag（待定）

首次對外可消費版本。包含：
- `club.common.v1`：`ErrorCode` / `Ack` / `Wallet` / `UserInfo`
- `club.game.v1`：`GameService`（CreateRoom / EnterRoom / LeaveRoom / PlaceBet / Settle / Subscribe）、`RoomSnapshot` / `Seat` / `Payout` / `ChatMessage` / `GameState`
- `club.tx.v1`：`TxService`（Transfer / GetBalance / QueryRecords）、`TxEntry` / `TxType`
- `club.gw.v1`：`Envelope` / `HelloRequest` / `HelloResponse` / `Ping` / `Pong` / `Kick` / `KickReason`
- `club.record.v1`：`RecordService`（WriteGameRecord / QueryGameRecords）、`GameRecord`

Tag 切法：從 monorepo `main` 鏡像推送到獨立 repo `github.com/club8/pkg-proto` 後打 tag；外部消費者 `go get github.com/club8/pkg-proto@v1.0.0` 即可。
