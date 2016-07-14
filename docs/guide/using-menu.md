菜单用法
==========

Menu 管理用于构建多级菜单。
它会通过用户权限来显示指定的菜单。

```php
use thinkwill\admin\components\MenuHelper;
use yii\bootstrap\Nav;

echo Nav::widget([
    'items' => MenuHelper::getAssignedMenu(Yii::$app->user->id)
]);
```

通过 `thinkwill\admin\components\MenuHelper::getAssignedMenu()` 函数返回数组格式如下:

```php
[
    [
        'label' => $menu['name'], 
        'url' => [$menu['route']],
        'items' => [
			[
				'label' => $menu['name'], 
				'url' => [$menu['route']]
            ],
            ....
        ]
    ],
    [
        'label' => $menu['name'], 
        'url' => [$menu['route']],
        'items' => [
			[
				'label' => $menu['name'], 
				'url' => [$menu['route']]
            ]
        ]
    ],
    ....
]
```

在`$menu`变量对应于一个数据表的菜单。
您可以自定义返回`thinkwill\admin\components\MenuHelper::getAssignedMenu()`的通过这种方法返回一个数组。
返回格式为`function($menu){}`。
例如:
    您可以添加html选项到您的菜单，例如"title"。
    当你创建一个菜单，在可以这样配置：``` return ['title'=>'Title of Your Link Here']; ```

然后在你的视图里：

```php
$callback = function($menu){
    $data = eval($menu['data']);
    return [
        'label' => $menu['name'], 
        'url' => [$menu['route']],
        'options' => $data,
        'items' => $menu['children']
    ];
}

$items = MenuHelper::getAssignedMenu(Yii::$app->user->id, null, $callback);
```

默认结果是从缓存。如果要强制再生，可以将第四个参数设置为 `true`。

您可以配置高级的回调函数。

![列表菜单](/docs/images/image09.png)
![创建菜单](/docs/images/image10.png)

使用下拉菜单
-------------------
`thinkwill\admin\components\MenuHelper::getAssignedMenu()`也可以创建下拉菜单.
例如您的菜单结构:

* Side Menu
  * Menu 1
    * Menu 1.1
    * Menu 1.2
    * Menu 1.3
  * Menu 2
    * Menu 2.1
    * Menu 2.2
    * Menu 2.3
* Top Menu
  * Top Menu 1
    * Top Menu 1.1
    * Top Menu 1.2
    * Top Menu 1.3
  * Top Menu 2
  * Top Menu 3
  * Top Menu 4

你可以通过 'Side Menu'继承调用

```php
$items = MenuHelper::getAssignedMenu(Yii::$app->user->id, $sideMenuId);
```

它将会返回
* Menu 1
  * Menu 1.1
  * Menu 1.2
  * Menu 1.3
* Menu 2
  * Menu 2.1
  * Menu 2.2
  * Menu 2.3

菜单过滤
--------------
如果你有`NavBar`导航栏菜单项和要判断用户登录是否登录。您可以使用Helper class。

```php
<?php
user thinkwill\admin\components\Helper;

$menuItems = [
    ['label' => 'Home', 'url' => ['/site/index']],
    ['label' => 'About', 'url' => ['/site/about']],
    ['label' => 'Contact', 'url' => ['/site/contact']],
    ['label' => 'Login', 'url' => ['/user/login']],
    [
        'label' => 'Logout (' . \Yii::$app->user->identity->username . ')',
        'url' => ['/user/logout'],
        'linkOptions' => ['data-method' => 'post']
    ],
    ['label' => 'App', 'items' => [
        ['label' => 'New Sales', 'url' => ['/sales/pos']],
        ['label' => 'New Purchase', 'url' => ['/purchase/create']],
        ['label' => 'GR', 'url' => ['/movement/create', 'type' => 'receive']],
        ['label' => 'GI', 'url' => ['/movement/create', 'type' => 'issue']],
    ]]
];

$menuItems = Helper::filter($menuItems);

echo Nav::widget([
    'options' => ['class' => 'navbar-nav navbar-right'],
    'items' => $menuItems,
]);
```

您还可以检查单个路由。

```php
use thinkwill\admin\components\Helper;

if(Helper::checkRoute('delete')){
    echo Html::a(Yii::t('rbac-admin', '删除'), ['delete', 'id' => $model->name], [
        'class' => 'btn btn-danger',
        'data-confirm' => Yii::t('rbac-admin', '你确定要删除此项?'),
        'data-method' => 'post',
    ]);
}
```

过滤按钮操作
---------------------------
当您使用 `GridView`, 您可以过滤您的按钮.

```php
use thinkwill\admin\components\Helper;

'columns' => [
    ...
    [
        'class' => 'yii\grid\ActionColumn',
        'template' => Helper::filterActionColumn('{view}{delete}{posting}'),
    ]
]
```
它会自动检查权限进行显示/隐藏。

更多...
---------------

- [**用户管理**](user-management.md)
- [**基本用法**](basic-usage.md)
- [**基本配置**](configuration.md)
