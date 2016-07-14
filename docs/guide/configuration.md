基本配置
---------

第一次安装完成后, 修改您的应用程序配置如下:

```php
return [
    'modules' => [
        'admin' => [
            'class' => 'thinkwill\admin\Module',
            ...
        ]
        ...
    ],
    ...
    'components' => [
        ...
        'authManager' => [
            'class' => 'yii\rbac\PhpManager', // 使用您的'yii\rbac\DbManager'
        ]
    ],
    'as access' => [
        'class' => 'thinkwill\admin\components\AccessControl',
        'allowActions' => [
            'site/*',
            'admin/*',
            'some-controller/some-action',
            // 这里列出的访问路径将被允许给每个访问者，包括guest。
            // 所以, 'admin/*' 不应该出现在这里正式版.
            // 但在你开发的早期阶段，你可能会需要添加许多访问路径用来开发您的项目。
            //否则您可能无法访问相应的功能。
        ]
    ],
];
```
请查看[Yii RBAC](http://www.yiichina.com/doc/guide/2.0/security-authorization)了解更多.
你可以通过以下网址访问权限管理系统:

```
http://localhost/path/to/index.php?r=admin
http://localhost/path/to/index.php?r=admin/route
http://localhost/path/to/index.php?r=admin/permission
http://localhost/path/to/index.php?r=admin/menu
http://localhost/path/to/index.php?r=admin/role
http://localhost/path/to/index.php?r=admin/assignment
http://localhost/path/to/index.php?r=admin/user
```

要使用菜单管理器（可选），使用下面语句执行迁移:
```
yii migrate --migrationPath=@thinkwill/admin/migrations
```

如果您的数据库 (class 'yii\rbac\DbManager') 已经创建了RBAC数据, 使用下面语句执行迁移:
```
yii migrate --migrationPath=@yii/rbac/migrations
```

配置 Assignment Controller
---------------------------------

Assignment controller 属性需要配置到您的 User 模型.
我们通过改变`controllerMap` 配置来达到目的.
如:

```php
    'modules' => [
        'admin' => [
            ...
            'controllerMap' => [
                 'assignment' => [
                    'class' => 'thinkwill\admin\controllers\AssignmentController',
                    /* 'userClassName' => 'app\models\User', */
                    'idField' => 'user_id',
                    'usernameField' => 'username',
                    'fullnameField' => 'profile.full_name',
                    'extraColumns' => [
                        [
                            'attribute' => 'full_name',
                            'label' => 'Full Name',
                            'value' => function($model, $key, $index, $column) {
                                return $model->profile->full_name;
                            },
                        ],
                        [
                            'attribute' => 'dept_name',
                            'label' => 'Department',
                            'value' => function($model, $key, $index, $column) {
                                return $model->profile->dept->name;
                            },
                        ],
                        [
                            'attribute' => 'post_name',
                            'label' => 'Post',
                            'value' => function($model, $key, $index, $column) {
                                return $model->profile->post->name;
                            },
                        ],
                    ],
                    'searchClass' => 'app\models\UserSearch'
                ],
            ],
            ...
        ]
        ...
    ],

```

- 系统要求
    - **userClassName** User模型完整的类名
        通常，您不需要特定地配置它，因为该模块将自动检测它
    - **idField** User模型的标识字段
        相当于 Yii::$app->user->id.
        默认值为 'id'.
    - **usernameField** User模型用户名字段
        默认值为 'username'.
- 可选属性
    - **fullnameField** 在 "view" 页面中使用用户全名字段.
        这可以是User模型的一个字段或一个相关模型（例如，profile模型）。
        当该字段是一个相关的模型，名称应以点分隔符号指定（例如 'profile.full_name'）。
        默认值为空。
    - **extraColumns** 在 "index" 页面使用的额外的字段
        这应该是一个 'view' 列的定义的数组。
        您可以包括相关模型的属性，如在上面的示例中所见。
        默认值是一个空数组。
    - **searchClass** 用于搜索User模型的搜索类名称
        您必须提供相应的搜索模型，以启用额外的列的筛选和排序。
        默认值为空。


自定义页面布局
--------------

默认情况下，管理模块将使用在应用程序级别中指定的布局。
在这种情况下，你可以自定义您所想要的页面布局。

通过指定`layout`属性，您可以使用该模块的内置布局之一：
'left-menu', 'right-menu' 或 'top-menu',您可以使用以下数组配置到模块。

```php
    'modules' => [
        'admin' => [
            ...
            'layout' => 'left-menu', // 默认为空，使用应用程序的布局没有菜单
                                     // 其他可用的值是'right-menu'和'top-menu'
        ],
        ...
    ],
```

如果你使用其中的一个，你也可以自定义菜单。您可以更改菜单标签或禁用它。

```php
    'modules' => [
        'admin' => [
            ...
            'layout' => 'left-menu',
            'menus' => [
                'assignment' => [
                    'label' => 'Grant Access' // label 设置
                ],
                'route' => null, // 隐藏menu
            ],
        ],
        ...
    ],
```



当使用一个专用的模块布局，您可能仍然希望它包裹在您的应用程序的主要布局有你的应用程序的导航栏和你的品牌Logo。
你可以通过指定` mainLayout `特性与应用的主要布局。例如:


```php
    'modules' => [
        'admin' => [
            ...
            'layout' => 'left-menu',
            'mainLayout' => '@app/views/layouts/main.php',
            ...
        ],
        ...
    ],
```

更多...
---------------

- [**基本用法 ...**](basic-usage.md)
- [**用户管理 **](user-management.md)
- [**菜单使用**](using-menu.md)
