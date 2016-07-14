YII2 RBAC通用管理后台
======================
YII2 RABC (Role Base Access Control)图形化管理系统. 轻松管理您的用户 :smile:.

文档
----

- [更新日志](CHANGELOG.md).
- [授权向导 ](http://www.yiichina.com/doc/guide/2.0/security-authorization). 提示：进行下一步前请先阅读.
- [基本配置](docs/guide/configuration.md)
- [使用方法](docs/guide/basic-usage.md).
- [用户管理](docs/guide/user-management.md).
- [菜单管理](docs/guide/using-menu.md).

安装
----

###Composer安装

如果您未安装 [composer](http://getcomposer.org/download/)，请先安装.

然后在命令行执行:

```
php composer.phar require thinkwill/yii2-admin "~1.0"

然后，添加

```
"thinkwill/yii2-admin": "~1.0"
```

到您的 `composer.json`文件并执行`php composer.phar update`语句.

### 归档安装

下载最新版本的程序[releases](https://github.com/thinkwill/yii2-admin/releases), 放到您的项目中.
如需配置请按以下格式进行配置

```php
return [
    ...
    'aliases' => [
        '@thinkwill/admin' => 'path/to/your/extracted',
        // for example: '@thinkwill/admin' => '@app/extensions/thinkwill/yii2-admin',
        ...
    ]
];
```

[**更多...**](docs/guide/configuration.md)

