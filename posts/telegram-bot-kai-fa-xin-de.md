---
title: 'Telegram Bot 開發心得'
date: 2022-07-25 17:03:49
tags: [program]
published: true
hideInList: false
feature: 
isTop: false
---
用telegram的bot寫了點小工具，
用Webhook的話記得php那邊要這樣接資料
```php=
// Takes raw data from the request
$json = file_get_contents('php://input');
// Converts it into a PHP object
$lastMsg = json_decode($json, true);
```
測試的時候用getUpdates
getUpdate version
```php=
 $url = "https://api.telegram.org/bot.../getUpdates";
 $s = json_decode(file_get_contents($url), true);
 $msgList = [];
 foreach ($s['result'] as $v) {
     $msgList[] = $v;
 }
 $lastMsg = $msgList[count($msgList) - 1];
 ```
 #用webhook就不能用getUpdates

 看Webhook有沒有異常，pending_updates不是0的話表示有異常訊息
https://api.telegram.org/bot.../getWebhookInfo

清空pending_updates
https://api.telegram.org/bot.../setWebhook?url=webhook_url&drop_pending_updates=true

啟動webhook
https://api.telegram.org/bot.../setWebhook?url=webhook_url

關閉 webhook
https://api.telegram.org/bot.../setWebhook?url=

希望bot是自己私人用的，就在webhook的php那邊加上判斷來源id的部分
```php=
$allowID = [
    "ID_1",
    "ID_2"
];
if(!in_array($lastMsg['message']['from']['id'],$allowID)) return false;
```

因為webhook不太好看有沒有記錄傳進來，所以我用了個資料表專門記錄傳進來的資料

因為telegram的伺服器會有時差，連db的設定檔可以多這句
```php=
$GLOBALS['pdo']->prepare("SET time_zone = 'Asia/Taipei'");
```
程式內若有用到DateTime()，也可以靠這句改時區
```php=
$dt = new DateTime('now', new DateTimeZone('Asia/Taipei'));
```