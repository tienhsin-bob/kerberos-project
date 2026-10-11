# Kerberos Authentication and Public-Key Distribution

本專案是資安課程期末專案，以 Python 和 Tkinter 製作圖形化展示程式，模擬 Kerberos 認證流程，並展示時間戳記檢查、重放攻擊情境，以及 RSA 數位簽章與加解密。

## 專案內容

- `kerberos.py`：Kerberos 認證流程與 RSA 示範的圖形化程式。
- 展示影片：[觀看專案展示影片](https://youtu.be/wIH8UqB8iuI)

## 程式功能

### 1. Kerberos 認證流程模擬

程式以 Tkinter 建立操作介面，透過按鈕逐步展示 Client、Authentication Server（AS）、Ticket-Granting Server（TGS）與 Application Server 之間的認證流程：

1. Client 輸入使用者 ID 與密碼。
2. Client 向 AS 請求認證。
3. AS 產生 Client 與 TGS 共用的 Session Key，並建立 Ticket-Granting Ticket（TGT）。
4. Client 使用密碼衍生出的金鑰解密 AS 回應，取得 Session Key 與 TGT。
5. Client 將 TGT 提交給 TGS，申請目標服務的 Service Ticket。
6. TGS 驗證 TGT，產生 Client 與 Application Server 共用的 Session Key，並建立 Service Ticket。
7. Client 將 Service Ticket 提交給 Application Server。
8. Application Server 驗證票據，並以時間戳記加一的回應示範雙向認證。

介面會顯示目前流程階段與執行日誌，方便觀察各角色處理的訊息。

### 2. 重放攻擊情境展示

程式提供「模擬重放攻擊」選項。啟用後，程式會將 TGT 中的時間戳記設為目前時間的 10 分鐘以前。TGS 會檢查時間戳記是否超過 300 秒的允許誤差；若超過，便拒絕繼續認證並在日誌中顯示警示。

### 3. RSA 數位簽章與加解密示範

完成 Kerberos 流程後，程式會另外執行 RSA 示範：

- 產生 2048 位元 RSA 公私鑰。
- 使用 SHA-256 與 RSA PKCS#1 v1.5 對測試訊息簽章，並驗證簽章。
- 使用 RSA-OAEP 公鑰加密測試訊息，再以私鑰解密，確認內容一致。

這部分是獨立的密碼學示範，不代表 RSA 簽章或 RSA 加密是前面 Kerberos 認證流程的一部分。

## 使用技術

- Python
- Tkinter：圖形化操作介面
- PyCryptodome：
  - AES：模擬票據與訊息加解密
  - RSA-OAEP：公鑰加密與私鑰解密
  - RSA PKCS#1 v1.5、SHA-256：數位簽章與驗證
  - 隨機金鑰產生與資料填充

## 執行方式

### 環境需求

- Python 3
- Tkinter
- PyCryptodome

安裝 PyCryptodome：

```bash
python -m pip install pycryptodome
```

執行程式：

```bash
python kerberos.py
```

程式啟動後，輸入使用者 ID 與密碼，再按「開始認證流程」。每按一次按鈕，程式就會進入下一個認證階段。要觀察重放攻擊情境，可在開始前勾選「模擬重放攻擊」。

## 測試帳號

目前程式內以固定資料模擬使用者資料庫：

- 使用者 ID：`group1`
- 密碼：`abc123`

此帳密僅供本機課堂展示使用，請勿用於任何真實服務或帳號。

## 專案限制與安全說明

本程式的目標是展示概念與流程，並非正式 Kerberos 實作，也不應用於真實環境。程式中包含教學用簡化設計，例如：

- AS、TGS 與 Application Server 以同一個本機程式模擬，沒有實際網路通訊或伺服器隔離。
- AES 使用 ECB 模式；此模式不適合保護真實通訊資料。
- 密碼以 SHA-256 雜湊後直接截取部分位元組作為 AES 金鑰，沒有使用專門的密碼衍生函式與隨機 Salt。
- 使用固定測試帳密與伺服器金鑰。
- 時間戳記檢查僅用來展示過期訊息的拒絕流程，沒有實作完整的票據生命週期或已使用票據追蹤。

因此，本程式僅供課堂報告、流程展示與學習使用，不應作為正式認證系統或安全通訊方案。

## 展示影片

[觀看 Kerberos 專案展示影片](https://youtu.be/wIH8UqB8iuI)
