用户管理
===============

对于 `基本应用程序模板` 要将用户数据存储在数据库当中.
要使用此功能，通过执行迁移创建所需的表。
```
./yii migrate --migrationPath=@thinkwill/admin/migrations
```
然后，配置程序user属性
```php
    'components' => [
        ...
        'user' => [
            'identityClass' => 'thinkwill\admin\models\User',
            'loginUrl' => ['admin/user/login'],
        ]
    ]
```
这样你就可以在菜单访问`index.php?r=admin/user`路由.


注册用户
-----------
```
http://localhost/myapp/index.php?r=admin/user/signup
```
默认注册用户状态为`ACTIVE`，意味着用户可以登录，无需激活。
为改变这一情况，你可在`config/params.php`配置
```php
// config/params.php

return [
    ...
    'thinkwill.admin.configs' => [
        'defaultUserStatus' => 0, // 0 = inactive, 10 = active
    ]
];
```

登录页面
----------
您可以通过访问`index.php?r=admin/user/login`进行登录操作。

更多...
---------------

- [**基本用法**](basic-usage.md)
- [**菜单使用**](using-menu.md)
- [**基本配置**](configuration.md)
