---
title: nodejs
date: 2017-09-15 16:26:06
tags: nodejs
---

get練習
---

views/login.html

<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <title>Login</title>
</head>
<body>
    <form>
        <label for="username">Username:</label>
        <input type="text" id="username">
        <label for="password">Password:</label>
        <input type="password" id="password">
        <button>送出</button>
    </form>
</body>
</html>


//最上方引入
const path = require("path");

//拿網頁或取得資料用get
app.get("/login", function(req, res){
    res.sendFile(path.resolve(__dirname, "./views/login.html"));
});
sendFile: 傳送一個實體的檔案
path.resolve: 將相對目錄轉絕對目錄
__dirname: 代表根目錄

處理傳送資料

安裝body-parser：

npm install body-parser --save
index.js

練習接收client傳來的資料，在傳回去


//在最上方引入
const bodyParser = require("body-parser");

//註冊 body parser middleware
app.use(bodyParser.json());    //處理json格式的資料
app.use(bodyParser.urlencoded({extended: false}));    //處理網址列所帶的參數

//因為是post，所以不能直接在網址輸入localhost:3000/login測試
app.post("/login", function(req, res){
    //傳入資料會存在 req.body 中
    console.log(req.body);
    //以 json 格式回傳給客戶端
    res.json(req.body);
});


用token紀錄狀態
---

如果沒有記錄狀態，網頁不會你已經登入過了，你可以再次進入到登入頁面

不要在裡面放入敏感資訊（例如：密碼）

快速產生token

npm install jsonwebtoken --save 
utils/jwt.js

可能會有很多檔案都要使用到JWT，所以獨立一個檔案，將程式碼分離做更好的使用！


const jwt = require("jsonwebtoken");
const key = "asd9uj8asld3";    //密鑰自己設

// 在server端註冊token
function signToken(payload, callback){
    jwt.sign(payload, key, callback);
}

//匯出，其他檔案才能引入
module.exports = {
    signToken: signToken
}
補充說明：
jwt.sign這個 function 會把 payload(在此為使用者帳號) 做成 token

再把 token 傳回使用者這邊，使用者收到 token 後會把它儲存在瀏覽器中(localStorage)

之後使用者透過瀏覽器登入 server 時，會把存在 localStorage 中的 token 傳給 server 驗證，server 透過 jwt.verify 這個 function ，將 token 和 key (sign的時候用的key)反轉成使用者帳號，驗證就會通過。

---
