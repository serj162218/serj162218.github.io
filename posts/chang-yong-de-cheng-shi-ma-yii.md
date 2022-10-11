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

//Criteria
$criteria = new CDbCriteria;
$criteria->addBetweenCondition('t.created_at', $dateStart, $dateEnd);
$dataProvider = new CActiveDataProvider('tablename', [
    'criteria' => $criteria,
    'sort' => array(
        'attributes' => array(
            '*'
        ),
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

view的CGridView直接用relations
```php=
[
    'header' => 'id',
    'value' => '$data->relations->id,
],
```

用toggle快速改狀態
```php=

<?php
Yii::app()->clientScript->registerCssFile('/include/bootstrap-toggle/css/bootstrap2-toggle.min.css');
Yii::app()->clientScript->registerScriptFile('/include/bootstrap-toggle/js/bootstrap2-toggle.min.js');

//GridView
[
    'header' => '已發送',
    'type' => 'raw',
    'value' => function ($data)
    {
        $check = ($data->paid == 0) ? '' : 'checked';
        return "<input {$check} class='active-btn' onchange='toggleActive({$data->id})' data-toggle='toggle' data-on='Y' data-off='N' data-onstyle='success' data-offstyle='danger' type='checkbox'>";
    },
],
?>
<script type="text/javascript">
	// 修改狀態
	function toggleActive(id) {
		$.ajax({
		    url: "<?= $this->createUrl('ToggleActive'); ?>",
		    type: "POST",
		    dataType: "JSON",
		    data: {
		    	id:id
		    },
		    success: function (response) {
		        alert((response.status == 1) ? '修改成功' : response.msg);
		    },
		    error: function (XMLHttpRequest, textStatus, errorThrown) {
		        alert(XMLHttpRequest.readyState + XMLHttpRequest.status + XMLHttpRequest.responseText);
		    },
		});
	}
</script>

//controller的部分
<?php
    public function actionToggleActive()
    {
        $request = app()->request;
        if ($request->isPostRequest) {
            try {
                $id = $request->getPost('id');
                if (!isset($id))
                    throw new Exception('Error', 1);

                $model = Member::model()->findByPk($id);
                if (!$model)
                    throw new Exception('查無資料', 1);

                $model->status = ($model->status == 0) ? 1 : 0;
                $model->saveAttributes(['status']);

                echo json_encode([
                    'status' => 1,
                ]);
            } catch (Exception $e) {
                echo json_encode([
                    'status' => 0,
                    'msg' => $e->getMessage(),
                ]);
            }
        }
    }
    ?>
```

accessRule
```php=
<?php
    public function accessRules()
    {
        return array(
            [
                //所有會員
                'allow',
                'actions' => ['a'],
                'users' => ['*'],
            ],
            [
                //登入的會員無法到登入、註冊
                'deny',
                'actions' => ['login', 'register'],
                'users' => ['@'],
                'deniedCallback' => function () {
                    Yii::app()->controller->redirect(['/site/index']);
                },
            ],
            [
                //expression，誰適用於這個規則，配deniedCallback
                'deny',
                'users' => ['@'],
                'expression' => [$this, 'isStatusNotAllowed'],
                'deniedCallback' => function () {
                    Yii::app()->controller->redirect(['/site/index']);
                },
            ],
            [
                //expression，也可用在allow
                'allow',
                'actions' => ['b'],
                'users' => ['@'],
                'expression' => [$this, 'isStatusAllow'],
            ],
            [
                'allow',
                'users' => ['@'],
            ],
            [
                //未登入的會員
                'allow',
                'actions' => ['login', 'register'],
                'users' => ['?'],
            ],
            [
                //deny所有，做安全保障
                'deny',
                'users' => ['*'],
                'deniedCallback' => function () {
                    Yii::app()->controller->redirect(['/site/login']);
                },
            ],
        );
    }

    protected function isStatusNotAllowed()
    {
        if (app()->user->isGuest) return false;
        $notAllowed = [...];
        $member = Member::model()->findByAttributes(['id' => app()->user->id]);
        return in_array($member->status, $notAllowed);
    }

    protected function isStatusAllow()
    {
        $member = Member::model()->findByAttributes(['id' => app()->user->id]);
        return !$member->status == Member::STATUS_ALLOW;
    }
?>
```