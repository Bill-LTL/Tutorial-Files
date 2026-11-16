# CS2／Gamescope：巢狀 XWayland 的 XIM

Gamescope 可以建立自己的巢狀 XWayland 顯示。桌面 XWayland 上已經有 XIM 註冊，不代表 Gamescope 使用的另一個 XWayland 顯示也已註冊。這份腳本會尋找 Gamescope 的 XWayland 顯示，並在啟動遊戲命令前確認 Fcitx5 的 XIM 已經連上該顯示。

## 腳本

完整腳本：[cs2-with-fcitx-xim](./cs2-with-fcitx-xim)

## 安裝與使用

先確認有 Bash、`ps`、`awk`、`xprop`、`fcitx5-remote`，且 Fcitx5 已在桌面工作階段執行。

將腳本複製到使用者程式目錄並設為可執行：

```bash
mkdir -p ~/.local/bin
cp cs2-with-fcitx-xim ~/.local/bin/cs2-with-fcitx-xim
chmod +x ~/.local/bin/cs2-with-fcitx-xim
```

在 Steam 的 CS2「內容 → 一般 → 啟動選項」中，將啟動命令設為以下形式：

```text
/home/你的使用者名稱/.local/bin/cs2-with-fcitx-xim %command%
```

將路徑換成你實際安裝腳本的位置。若原本已有 Gamescope 或其他啟動參數，請依 Steam 啟動選項格式保留原有設定，避免覆蓋掉其他必要參數。

## 腳本執行流程

1. 短暫輪詢 XWayland 程序，尋找 Gamescope 的巢狀顯示。
2. 確認該顯示可連線，並確認 Fcitx5 正在執行。
3. 若該顯示尚未註冊 Fcitx XIM，要求 Fcitx5 連線並等待註冊。
4. 註冊成功後執行 Steam 傳入的遊戲命令；失敗時印出錯誤並停止。

## 限制與還原

- 腳本依賴系統的 XWayland 程序參數格式來辨識巢狀顯示；Gamescope 或 XWayland 實作改變時可能需要調整。
- 它只處理遊戲啟動前的 XIM 連線，不保證 Gamescope 內的候選字視窗一定可見。
- 若要還原，在 Steam 的 CS2 啟動選項中移除包裝腳本，恢復原本的啟動命令即可。
