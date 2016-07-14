权限分配
---------
权限分配用于授予或撤销用户角色。

![权限分配](/docs/images/image01.png)
![权限分配](/docs/images/image02.png)

路由
-----
路由是特别的许可。其与应用路线。因为路由是允许的，
所以你可以把它分配到另一个许可或角色。
默认情况下，列出的路由是从应用程序结构自动读取的。
单击按钮 '>>'以保存它，单击按钮 '<<'删除。

如果你需要的路由在列表中不存在。您可以手动创建它。
您还可以创建路由附加参数。使用`&`设置路由的主要参数。例如：`site/page&view=about`。

![路由](/docs/images/image03.png)

角色和权限
-------------------

这部分用于管理角色/权限。您可以创建、删除或更新此菜单中的角色/权限。
添加和删除角色的子角色也可以在那里做。


![角色](/docs/images/image04.png)
![创建角色](/docs/images/image05.png)
![添加子角色](/docs/images/image06.png)
![更新权限](/docs/images/image07.png)

规则
----

要使用规则，定义你自己的规则类。它应该继承
[`yii\rbac\Rule`](http://www.yiiframework.com/doc-2.0/yii-rbac-rule.html).
查看[Rules](http://www.yiiframework.com/doc-2.0/guide-security-authorization.html#using-rules)相应文档。
添加规则到`authManager`。

![规则](/docs/images/image08.png)

更多...
--------

- [**用户管理**](user-management.md)
- [**菜单使用**](using-menu.md)
- [**基本配置**](configuration.md)

