# Render 部署說明

## 專案概述
護理人員排班系統已針對Render平台進行優化，支援雲端部署與資料持久化。

## 部署前準備

### 1. 確保專案結構
```
NurseScheduler2RenderDemo/
├── app.py                 # 主應用程式
├── requirements.txt       # Python依賴
├── render.yaml           # Render部署配置
├── gunicorn.conf.py      # Gunicorn配置
├── templates/            # HTML模板
├── static/              # 靜態檔案
└── data/                # 本地開發資料夾
```

### 2. 重要配置變更
- ✅ 資料庫路徑：`data/` → `/var/data/`
- ✅ 新增Gunicorn支援
- ✅ 環境變數配置
- ✅ 持久化磁碟掛載

## Render部署步驟

### 1. 連接GitHub倉庫
1. 登入 [Render Dashboard](https://dashboard.render.com/)
2. 點擊「New +」→「Web Service」
3. 選擇您的GitHub倉庫：`DavidLin729/NurseScheduler2RenderDemo`

### 2. 配置部署設定
- **Name**: `nurse-scheduler` (或您喜歡的名稱)
- **Environment**: `Python 3`
- **Build Command**: `pip install -r requirements.txt`
- **Start Command**: `gunicorn app:app`
- **Plan**: `Free` (或選擇付費方案)

### 3. 環境變數設定
在Render Dashboard中設定以下環境變數：
```
FLASK_ENV=production
PYTHON_VERSION=3.9.16
```

### 4. 持久化磁碟配置
1. 在服務設定中新增磁碟
2. **Name**: `data-disk`
3. **Mount Path**: `/var/data`
4. **Size**: `1 GB` (免費方案上限)

### 5. 部署
點擊「Create Web Service」開始部署

## 部署後配置

### 1. 資料庫初始化
首次部署後，系統會自動：
- 建立資料庫檔案在 `/var/data/staff.db`
- 初始化所有資料表
- 建立預設管理員帳號

### 2. 預設登入資訊
- **帳號**: `admin`
- **密碼**: `admin123`
- **建議**: 首次登入後立即修改密碼

### 3. 資料備份
- 資料庫檔案位於 `/var/data/staff.db`
- 建議定期備份此檔案
- 可透過Render Dashboard下載

## 本地開發 vs 生產環境

### 本地開發
```bash
# 資料庫路徑
data/staff.db

# 啟動命令
python app.py

# 訪問地址
http://127.0.0.1:5001
```

### 生產環境 (Render)
```bash
# 資料庫路徑
/var/data/staff.db

# 啟動命令
gunicorn app:app

# 訪問地址
https://your-app-name.onrender.com
```

## 故障排除

### 1. 部署失敗
- 檢查 `requirements.txt` 是否包含所有依賴
- 確認 `render.yaml` 配置正確
- 查看Render部署日誌

### 2. 資料庫錯誤
- 確認磁碟已正確掛載到 `/var/data`
- 檢查資料庫檔案權限
- 重新部署以重新初始化資料庫

### 3. 應用程式無法啟動
- 檢查 `gunicorn.conf.py` 配置
- 確認環境變數設定
- 查看應用程式日誌

## 監控與維護

### 1. 日誌查看
- 在Render Dashboard查看即時日誌
- 監控錯誤和警告訊息
- 追蹤應用程式效能

### 2. 版本更新
- 推送新程式碼到GitHub
- Render會自動重新部署
- 資料庫資料會保留

### 3. 擴展建議
- 升級到付費方案以獲得更多資源
- 考慮使用外部資料庫服務
- 實施更完善的備份策略

## 安全注意事項

1. **修改預設密碼**: 首次登入後立即修改管理員密碼
2. **定期備份**: 定期下載資料庫檔案作為備份
3. **監控存取**: 定期檢查系統存取日誌
4. **更新依賴**: 定期更新Python套件以修補安全漏洞

## 支援與聯絡

如有部署問題，請：
1. 檢查Render部署日誌
2. 確認所有配置檔案正確
3. 參考本文件故障排除章節
4. 聯絡系統開發者 