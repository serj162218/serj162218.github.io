---
title: '常用的程式碼 - Yii'
date: 2022-07-13 11:37:14
tags: []
published: true
hideInList: false
feature: 
isTop: false
---

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