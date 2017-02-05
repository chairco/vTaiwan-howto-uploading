# vTaiwan-howto-uploading
vTaiwan 上稿系統教學
如何上稿流程：[教學影音](https://www.youtube.com/playlist?list=PLbf_J5xlMK0GlSQURSj0b_NsozBZsh-Tn)


### 軟體需求
1. node.js (6.9.0)
2. yarn


### 上稿流程與架構
分成三大系統 GH page, Talk Taiwan, vTaiwan 。

1. GH page 法案內容
在 github 上建立一個 repo 專案，並可將其建置成 gitbook 並依據法案名稱為網址路徑上傳 gitbook 內容到 g0v.io。

2. Talk vTaiwan
討論區，根據法案建立相對討論區。(目前尚未自動化)

3. vTaiwan
法案網站。將前導簡報引入網站 react.vtaiwan 的 master


## 以下為流程的詳細說明(對應上述流程 1,2,3)

### 建立 gitbook、設定參數與 deploy 到 g0v.io
建立 MD File，資料夾會有 package.json、SUMMARY.md、GLOSSARY.md、LICENSE.md


### 手動上稿至 talk.vTaiwan 與建立討論內容
因為目前無法自動上傳內容到 talk.vTaiwan 所以需要手動在網站建立相對應的討論區。


### 將簡報導入 react.vtaiwan 

commend：

`git grep naming`

`git grep -l naming`

`git grep -l naming | xargs echo vim`

秀出下述畫面，複製並且開始編輯：

```
vim script/gen-static-second-level.sh src/App/data/Nav.js src/Category/data/Category.js src/Proposal/data/Proposals.json src/ProposalList/ProposalList.jsx
```

依序 next 將 naming 內容複製一份並且修改成法案英文：

* 複製 row `naming\init` 並改為 `法案\init`。

* 複製 `path: '/naming/'` 並改為 `path: '/法案/'`。

* 複製下面內容並將 naming 等資訊修改。

```
"naming":
{
 "init" :
 {
     "gitbook_url": "https://g0v.github.io/naming-gitbook",
     "category_num": 122,
     "proposal_cht":"公司英文名稱登記",
     "category_cht":"討論"
 }
}
```


* category_num 是到 talk.vTaiwan 的法案內容網誌(例如：`https://talk.vtaiwan.tw/c/securitization.json`)取得 "category_id":126 (126 是範例) 要注意父或是子分類的 category_id，簡單判斷就是最小那個。


* 複製一段結構法案結構（如下）

```
"naming": {
 "title_cht": "公司英文名稱登記",
 "title_eng": "naming",
 "proposer_abbr_cht": "經濟部",
 "proposer_abbr_eng": "moea",
 "slides_url": "http://www.slideshare.net/vtaiwan/ss-64723713",
 "links": [
   {
     "title": "諮詢會議：公司英文名稱登記",
     "date": "November 10, 2016 19:00:00",
     "links": [
       {
         "title": "共筆",
         "link": "https://hackpad.com/20161110--KEmoTM5HU85"
       },
       {
         "title": "直播",
         "link": "https://www.youtube.com/watch?v=1hOzNZ8mEqc"
       },
       {
         "title": "紀錄",
         "link": "https://sayit.archive.tw/2016-11-10-vtaiwan-%E7%B7%9A%E4%B8%8A%E8%AB%AE%E8%A9%A2%E6%9C%83%E8%AD%B0%E5%85%AC%E5%8F%B6    8%E8%8B%B1%E6%96%87%E5%90%8D%E7%A8%B1%E7%99%BB%E8%A8%98"
       },
       {
         "title": "PDF",
         "link": "http://www.vtaiwan.org.tw/model.aspx?no=276"
       }
     ]
   }
 ], 
 "stages": [
   {
     "name": "討論",
     "gitbook_url": "https://g0v.github.io/naming-gitbook",
     "discourse_url": "https://talk.vtaiwan.tw",
     "category": "init",
     "start_date": "September 23, 2016 00:00:00",
     "end_date": "October 23, 2016 23:59:59",
     "category_num": 122
   },
   {
     "name": "意見徵集",
     "category": "collect",
     "start_date": "August 14, 2016 00:00:00",
     "end_date": "August 31, 2016 23:59:59",
     "slido_id": "m35dexjd"
   }
 ],
 "slides_image": "//image.slidesharecdn.com/vdaivufrrzy9skivhbbv-signature-a9a79ab853707247671f0b8482eb36218a9a0a3def76a839473eae1a3c2    ced520-poli-160805080126/95/-1-1024.jpg",
 "slides_embed_url": "//www.slideshare.net/slideshow/embed_code/key/GhiZZQrxroNwCC"
},
```

修改:
```
"title_cht": "公司英文名稱登記",
"title_eng": "naming",
"proposer_abbr_cht": "經濟部",
"proposer_abbr_eng": "moea",
"slides_url": "http://www.slideshare.net/vtaiwan/ss-64723713",
```

如果尚未舉辦公聽會，`"links":[]` 請清空。
因為範例沒有第 0 階段只有一個 stage 所以只留一個，stage 的時間通常會用上稿後的那個凌晨起開始算，結束為一個月之後。


* 修改 icon 與連結錯誤

command:

找出法案的圖並且複製

1. `find .|grep securitiza`

2. `cp ./src/NavBar/images/securitization.png ./src/NavBar/images/moj.png`

取的新版年報，與正確簡報：

1. `less package.json`

2. `yarn run update`

執行以下指令似乎是 babel-node 沒有安裝好導致做了處理。

1. `ll node_modules/.bin/`

2. `yarn add babel-node`

3. `babel-node script/update-proposal-slides.js`


## build 靜態網頁

建立靜態網頁，注意如果是已經編譯過的頁面可以在 /script/build.sh 底下新增下面程式碼在進行編譯，這樣瀏覽器才會讀到新的頁面。
```
babel-node script/gen-static.js /social-enterprise/ 
```

指令兩個 1. 是用來建立頁面並且循環的去抓取與 discourse 連結的討論區內容。指令 2. 是用來本機測試，不要忘記先 `yarn run start` 先啟動 node 後端，static 是用來確認上道 gh-page 後網頁內容正確。
```
yarn build
yarn static
```

