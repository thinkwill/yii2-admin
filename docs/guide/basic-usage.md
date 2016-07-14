Admin模块
------------
- `layout` 默认为'left-menu'. 将其设置为'null'后，可使用您自身的布局.
- `menus` 可改变模块菜单

在模块中使用以下配置

```php
'modules' => [
    ...
    'admin' => [
        'class' => 'thinkwill\admin\Module',
        'layout' => 'left-menu', // 它可以被设置成 '@path/to/your/layout'.
        'controllerMap' => [
            'assignment' => [
                'class' => 'thinkwill\admin\controllers\AssignmentController',
                'userClassName' => 'app\models\User',
                'idField' => 'user_id'
            ],
            'other' => [
                'class' => 'path\to\OtherController', // 添加额外的Controller
            ],
        ],
        'menus' => [
            'assignment' => [
                'label' => 'Grand Access' // 标签配置
            ],
            'route' => null, // menu 路由
        ]
	],
],
```

Access Control Filter
---------------------

Access Control Filter (ACF)是一种简单的鉴权方式，是用来做一些简单的访问控制的一种好方法。
顾名思义，ACF是作为一个行为(behavior)附加在控制器(controller)或者一个模块(module)上的动作过滤器。
ACF会检查一组访问规则，以确认当前用户是否有足够的权限访问动作(action)。

以下代码展示了`thinkwill\admin\components\AccessControl`如何去使用ACF：


```php
'as access' => [
    'class' => 'thinkwill\admin\components\AccessControl',
    'allowActions' => [
        'site/login', 
        'site/error',
    ]
]
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

要检查route访问权限，你可以使用
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

更多...
---------------

- [**用户管理**](user-management.md)
- [**菜单使用**](using-menu.md)
- [**基本配置**](configuration.md)
