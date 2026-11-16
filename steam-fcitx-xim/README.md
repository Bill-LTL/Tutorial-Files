# Steam 中文輸入：Fcitx5／XIM 啟動腳本

## 問題現象

在 Arch Linux + KDE Plasma Wayland 環境中，Fcitx5／Rime 本身正常，但 Steam 聊天輸入框或部分 X11／XWayland 應用不能啟用中文輸入。這份腳本的用途，是等桌面 XWayland 與 Fcitx5 可用後，確認 Fcitx5 的 XIM 服務已在目前的 X 顯示上註冊，再啟動 Steam。

XIM 是 X11／XWayland 應用程式與輸入法溝通的一種方式。輸入法程序正在執行，不代表每個應用程式都已連上該輸入通道。

## 腳本

完整腳本：[steam-with-fcitx-xim](./steam-with-fcitx-xim)

腳本會：

1. 最多等待 90 秒，確認 `DISPLAY`、XWayland 與 Fcitx5 已就緒。
2. 檢查目前 X 顯示的根視窗是否已註冊 Fcitx XIM。
3. 若尚未註冊，嘗試連接 Fcitx5 並等待註冊完成。
4. 確認成功後才執行 `/usr/bin/steam`；逾時或必要條件不成立時會顯示錯誤並停止。

它不會修改全域環境變數，也不會常駐背景等待。若為連接 XIM 暫時建立了 `~/.Xauthority` 符號連結，腳本會在啟動 Steam 前清除自己建立的連結。

## 安裝與手動測試

先確認下列程式存在：`bash`、`xdpyinfo`、`xprop`、`fcitx5-remote`、`grep`，以及 `/usr/bin/steam`。腳本依賴目前桌面工作階段提供的 `DISPLAY`、XWayland 與 Fcitx5。

將腳本複製到自己的使用者程式目錄，例如：

```bash
mkdir -p ~/.local/bin
cp steam-with-fcitx-xim ~/.local/bin/steam-with-fcitx-xim
chmod +x ~/.local/bin/steam-with-fcitx-xim
```

先在已登入圖形桌面的終端機執行：

```bash
~/.local/bin/steam-with-fcitx-xim
```

確認 Steam 能正常啟動，並測試聊天輸入框的中文輸入。失敗時請查看終端機錯誤訊息；不要先刪除 Fcitx 設定或改動全域環境變數。

## 設定 Steam 自動啟動（不提供自動啟動檔）

自動啟動檔是每台電腦自己的使用者設定，因此本倉庫不提供或覆蓋該檔案。

修改位置：

```text
~/.config/autostart/steam.desktop
```

在 KDE「系統設定 → 自動啟動」中也可以管理這個項目。編輯既有 Steam 項目的執行命令（`Exec=`），讓它呼叫你安裝好的 `steam-with-fcitx-xim`，並保留原本需要的 `%U` 參數。請使用自己家目錄下腳本的實際絕對路徑；不要照抄其他使用者的 `/home/...` 路徑。修改前先備份原檔。

如果原本不是由這個 `.desktop` 檔啟動 Steam，請先確認實際使用的自動啟動項目，不要新增重複的 Steam 啟動項。

## 還原

在 KDE「系統設定 → 自動啟動」中，將 Steam 的執行命令改回原本值；或還原你事先備份的 `~/.config/autostart/steam.desktop`。這份教學不會要求修改 `/etc` 下的系統服務。

## 限制

- 這是針對使用 XWayland／XIM 的應用啟動問題所做的個人環境測試，不保證能修復所有 Steam、CEF、GTK 或 Wayland 輸入問題。
- 腳本使用 `/usr/bin/steam`，若 Steam 安裝位置不同，需相應調整腳本最後的執行路徑。
- 歷史上的 Fcitx5／Cairo 崩潰與目前 Steam 輸入問題是不同事件；本腳本不會修復或診斷 Cairo 崩潰。
