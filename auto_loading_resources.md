# 自动载入资源

CodeIgniter 的 “自动加载” 功能，可以允许系统每次运行时自动初始化类库，辅助函数和模型。如果你想让某些资源在系统中各处可用，可以考虑使用自动加载功能。

以下内容可以自动加载：

-  *libraries/* 文件夹里的类
-  *helpers/* 文件夹里的帮助文件
-  *config/* 文件夹里的自定义配置文件
-  *system/language/* 文件夹里的语言包
-  *models/* 文件夹里的模型

要自动加载，打开 **application/config/autoload.php** 文件并将他们添加到自动加载数组，你会发现该文件中对应于上面每个项目类型。

注意: 将内容添加到自动加载数组，不要添加 (.php) 扩展名。

另外，如果你想要让 CodeIgniter 使用 `Composer` 自动加载，可以设置  `$config['composer_autoload']` 为 `TRUE`，或者 **application/config/ 路径。
