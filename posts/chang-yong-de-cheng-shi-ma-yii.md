---
title: '常用的程式碼 - Yii'
date: 2022-07-13 11:37:14
tags: [常用的程式碼]
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

    //dataProvider by array data
    $dataProvider = new CArrayDataProvider($data, [
    'sort' => array(
        'defaultOrder' => 'id ASC',
        'attributes' => array(
            "id", "count" //可以點擊排序
        )
    ),
]);

```

Model 配 GridView 要用到FK的時候
在model內命名一個public $var;
model的rules的on => search前面加上要查詢的FK
```php=
array('id, content, type', 'safe', 'on' => 'search')
```
1. 在model內有用relations
```php=
$criteria->with = ['orig'];
$criteria->compare('orig.type', $this->type, true);
```
2. 在model內沒有用relations，或其他需要join的時候
```php=
$criteria->join = 'LEFT JOIN member b ON b.id = t.upper';
$criteria->compare('b.username', $this->upper_username, true);
```
在return的地方兩者都需要在attributes內加上排序用
```php=	
return new CActiveDataProvider('ModelName', array(
        'criteria' => $criteria,
        'sort' => array(
            'attributes' => array(
                'type' => array(
                    'asc' => 'orig.type',
                    'desc' => 'orig.type DESC',
                ),
                '*',
            ),
        ),
    ));
```
view的地方
為了可以查詢，要加上name
如果是用relations的話，沒有特別要寫html就不需要value了
```php=
[
    'name' => 'type',
    'type' => 'raw', //value return 的才會變html
    'header' => '類型',
    'headerHtmlOptions' => array("class" => 'span3'),
    'value' => function ($data)
    {
        return "<h3>".$data->orig->type."</h3>"
    }
],
```