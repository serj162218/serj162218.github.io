---
title: '常用的程式碼 - PHP'
date: 2022-07-07 14:09:00
tags: [program]
published: true
hideInList: false
feature: 
isTop: false
---

<h1>PHP</h1>

DateTime
```php=
    //拿現在時間
    $dt = new \DateTime('NOW');
    //拿月初 2022-07-01 00:00:00
    $dateStart = $dt->format('Y-m-01 00:00:00');
    //拿月底 2022-07-31 23:59:59
    $dateEnd = $dt->format('Y-m-t 23:59:59');
    //拿上個月月初
    $dateStart = date('Y-m-01 00:00:00', strtotime('-1 month'));
    //拿上個月月底
    $dateEnd = date('Y-m-t 23:59:59', strtotime($dateStart));
```

密碼驗證
```php=
    // 線上生成的都用這個，很好用
    // https://phppasswordhash.com/
    // 產生PWD
    password_hash($pwd, PASSWORD_DEFAULT)
    // 驗證用
    password_verify($pwd, $hash)，正確回傳true
    if (!password_verify($pwd, $hash))
        throw new Exception("錯誤");
```

Array
```php=
    //array_chunk($arr, $len, bool $preserve_keys) 切割陣列內元素成子陣列，preserve_keys = true會保留KEY
    $d = [1,2,3,4];
    print_r(array_chunk($d, 2));
    output: [[1,2], [3,4]]

    //implode(" ", $arr) Array to String connects with needles
    $d = [1,2,3,4];
    print_r(implode(" ", $d));
    output: 1 2 3 4

    //explode(" ", $str) String to Array split with needles
    $s = "1 2 3 4";
    print_r(explode(" ",$s));
    output: [1,2,3,4]
```