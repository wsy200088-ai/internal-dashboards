# internal-dashboards

內部儀表板靜態網站。**此 repo 必須維持 Private。**

## ⚠️ 重要:不要啟用 GitHub Pages

GitHub Pages 的個人站台(`wsy200088-ai.github.io`)**不論 repo 是否 Private,發布出來的網站一律對全世界公開**,而且會被搜尋引擎索引。私有 Pages 僅 GitHub Enterprise Cloud 才提供。

本 repo 一律透過 **Cloudflare Pages** 部署,並以 **Cloudflare Access** 控管存取。

```
GitHub (Private)  →  Cloudflare Pages  →  Cloudflare Access
     不對外              唯一入口           限公司 email
```

## 目錄

| 路徑 | 內容 |
|---|---|
| `fy26-target/index.html` | FY26 商售目標儀表板(期初 vs 期中) |

## Cloudflare Pages 設定

於 Cloudflare Dashboard → Workers & Pages → Create → Pages → Connect to Git:

| 欄位 | 值 |
|---|---|
| Framework preset | **None** |
| Build command | **(留空)** |
| Build output directory | **/** |
| Root directory | **(留空)** |

純靜態單檔,不需要任何建置流程。部署後網址為
`https://<專案名>.pages.dev/fy26-target/`

## Cloudflare Access 設定(務必做)

Zero Trust → Access → Applications → Add an application → Self-hosted

- Application domain:填上 Pages 的網域(含 `/fy26-target` 路徑亦可)
- Policy:Action = **Allow**,Include = **Emails ending in** `@benesse.com.tw`
  (或用 Emails 逐一列出總經理與各處長、組長)

免費方案含 50 位使用者。**沒設 Access 的話,知道網址的人都進得去。**

## 檔案本身的密碼

`fy26-target/index.html` 內的資料已用密碼衍生金鑰加密(SHA-256 迭代 10 萬次 + 串流加密),
未輸入正確密碼不會解密,檢視原始碼也看不到明文。

**但這是延緩,不是防護** —— 密文與演算法都在檔案裡,短密碼可離線暴力破解。
請把 Cloudflare Access 當作真正的防線,檔案密碼只當第二道門。

## 更新流程

1. 重新產生儀表板(產生器見下方)
2. 覆蓋 `fy26-target/index.html`
3. `git add . && git commit -m "update FY26 target dashboard" && git push`
4. Cloudflare Pages 自動重新部署,網址不變

## 儀表板的產生器

不在此 repo 內(含來源 Excel,不應上傳)。位置:

```
C:\D\2017\03.戰略組\04.新規集計\2026\目標\_儀表板產生器(claude)\
```

該資料夾的 `README.txt` 有完整重跑步驟。

## 待辦

- [ ] 舞台劇、樂園兩管道的**期中目標尚未定案**,定案後需重新產生儀表板
- [ ] 考慮將檔案密碼改為 16 字以上的隨機字串
