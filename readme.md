# yii框架api简单的实现

## 一
### (1)
先打开apache的路由重写
    在httpd.conf的大约154行将LoadModule rewrite_module modules/mod_rewrite.so的#号删除
    同时找到大约240行将AllowOverride none改为AllowOverride all

### (2)

    在web目录添加 .htaccess 文件 
    内容为
```
Options +FollowSymLinks
IndexIgnore */*

RewriteEngine on

# if a directory or a file exists, use it directly
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d

# otherwise forward it to index.php
RewriteRule . index.php

RewriteRule \.svn\/  /404.html
RewriteRule \.git\/  /404.html
```
## 二

### (1)
(2)修改配置文件

在api的config的文件下面，有一个boostrap.PHP的文件和main.php的主配置文件，因为这是我们新建的api应用，所以必须要设置api的别名。


在boostrap.php文件中添加：

Yii::setAlias('api', dirname(dirname(__DIR__)) . '/api');



main.php的主配置文件需要配置controllerNamespace属性


```
'controllerNamespace' => 'api\controllers',

'modules' => [
        'v1' => [
            'class' => 'api\modules\v1\Module'
        ]

],
```
main.php中路由的配置

```
'urlManager' => [  
    'enablePrettyUrl' => true,  
    'enableStrictParsing' => true,  
    'showScriptName' => false,  
    'rules' => [  
        [  
            'class' => 'yii\rest\UrlRule',  
            'controller' => ['v1/user']  
        ],  
    ],  
]  
```
### (2)

在数据库中建立一个user表

### (3)
   用过gii工具生成子模块 ***v1*** 
```
Module Class：api\modules\v1\Module
Module ID：v1
```
在v1的controller建立UserController.php类，继承自yii\rest\ActiveController

Usercontroller的内容:

```
<?php
	namespace api\modules\v1\controllers;

	use yii\rest\ActiveController;
	class UserController extends ActiveController
	{
		public $modelClass = 'api\modules\v1\models\User';	}

?>
```

在models建立叫User.php的文件

User.php内容:


```
<?php
namespace api\modules\v1\models;  


use Yii;  
use api\models\Order;  
use yii\db\ActiveRecord;  
class User extends ActiveRecord  
{  
  

  public function rules(){

  	return [
  		['id','string'],
  		['name','string'],
  		['pass','string']
  	];
  }
} 
?>
```
温馨提示：如果在User.php中不加上rules方法 可以正常的用get方法访问。但是用post或put方法时无法修改数据库内容





    

