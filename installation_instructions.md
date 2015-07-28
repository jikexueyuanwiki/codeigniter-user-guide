# 安装指南


CodeIgniter 的安装分为四个步骤:

* 解压缩安装包
* 把 CodeIgniter 文件夹和文件上传到你的服务器。通常把 index.php 放在根目录。
* 用任何文本编辑器打开 `application/config/config.php`, 并设置网站根 URL。如果你打算使用加密或 Session，需要设置加密密钥。
* 如果你打算使用数据库，用任何文本编辑器打开 `application/config/database.php`, 并设置数据库参数。

如果你希望通过隐藏 CodeIgniter 文件的路径来增加安全性，可以通过修改 system 和 application 目录的名字来实现，把它改成任何你喜欢的名字。如果已经修改了名字，你需要打开主目录下面的 index.php 文件设置里面的 `$system_path` 和 `$application_folder` 变量，把它改成之前修改的新名字。

为了安全考虑,system 和 application 两个文件夹应放到网站根目录（Web Root）以外的地方，这样浏览器就不能够直接访问它们。在默认设置下, 在每个文件夹中都有一个 `.htaccess` 配置文件以拒绝直接访问, 但是当把代码部署到生产环境时最好移除他们，因为生产环境的 Web 服务可能会不支持 `.htaccess` 的配置。


如果你移动了以上两个文件夹,请打开主目录下的 index.php 文件并编辑 $system_path 和$application_folder 两个变量, 最好使用绝对路径进行替换, 例如：`/www/MyUser/system`.

另外有一个附加的考虑就是，如果要在生产环境中使用，最好关闭 PHP 的错误报告,和其他任何与开发相关的功能，在 CodeIgniter 中，可以设置 `ENVIRONMENT` 常量来实现这个功能。详细的文档请参阅[安全]章节。

以上就是全部安装过程！

如果你刚刚接触 CodeIgniter，请阅读[用户指南]的开始部分，学习如何构造动态的 PHP 应用。让我们享受这个过程吧！

[安全]: security.md
[用户指南]: getting_started.md