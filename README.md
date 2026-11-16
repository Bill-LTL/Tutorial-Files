# Wayland 教學檔案

這個倉庫整理影片「Wayland 下中文輸入法失靈？Steam 輸入與遊戲滑鼠延遲一次排查」用到的腳本與操作說明。

> 內容依照個人的 Arch Linux + KDE Plasma Wayland 環境整理。不同發行版、Steam 版本、桌面環境與顯示伺服器的行為可能不同，請先閱讀各教學並自行確認環境，不要把這些步驟當成所有系統都適用的通用修復。

## 目錄

- [Steam：Fcitx5／XIM 啟動腳本](./steam-fcitx-xim/)
- [CS2／Gamescope：巢狀 XWayland 的 XIM 腳本](./cs2-gamescope/)
- [CS2：遊戲滑鼠延遲排查](./game-mouse-latency/)

## 包含哪些檔案

- `steam-fcitx-xim/steam-with-fcitx-xim`：完整 Steam 啟動包裝腳本。
- `cs2-gamescope/cs2-with-fcitx-xim`：完整 CS2／Gamescope 啟動包裝腳本。
- 各資料夾的 `README.md`：適用情境、使用方式、限制與還原說明。

Steam 自動啟動用的 `.desktop` 檔**不會放在這裡**。Steam 自動啟動的修改位置和修改方式，請看 [Steam／XIM 教學](./steam-fcitx-xim/README.md)。

## 注意事項

- 先在終端機手動測試腳本，再設定自動啟動或遊戲啟動選項。
- 腳本需要依實際安裝路徑調整時，請先確認路徑；不要直接照抄別人的家目錄路徑。
- 建議一次只改一項，並記錄修改前後的結果。
- 本倉庫不包含影片、字幕、個人自動啟動檔或使用者設定備份。
