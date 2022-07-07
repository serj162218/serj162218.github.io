---
title: '常用的程式碼紀錄'
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

Yii
```php=
    //SELECT
    Yii::app()->db->createCommand("SELECT * FROM test WHERE id = 1")->queryAll();
    //UPDATE, DELETE, INSERT
    Yii::app()->db->createCommand("...")->execute();

    //Model Search
    //By Attributes
    $member = Member::model()->findByAttributes(['role' => 1]);
    //Primary key, ex: id = 1
    $member = Member::model()->findByPk(1);

    //transaction start
    $transaction = Yii::app()->db->beginTransaction();
    //commit
    $transaction->commit();
    //rollback
    $transaction->rollback();

    //request
    $request = app()->request;
    //GET, default = 1
    $id = $request->getQuery('id', '1');
    //POST
    //is this from post
    if($request->isPostRequest){...}
    //getPost, default = 1
    $id = $request->getPost('id', '1');


```