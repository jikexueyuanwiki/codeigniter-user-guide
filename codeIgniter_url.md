# CodeIgniter URL

默认情况下，CodeIgniter 的 URL 设计成对搜索引擎和人类友好。不同于使用标准的“查询字符串”方法（它和动态系统同步），CodeIgniter 使用的是基于段的方法：

	example.com/news/article/my_article


`注意：查询字符串 URL 是可以选的，如下所示`

## URI 段

根据模型-视图-控制器模式，此 URL 段一般按以下形式表示：

	example.com/class/function/ID

1. 第一段表示调用控制器类
2. 第二段表示将要调用的类函数或类方法
3. 第三及更多的段表示的是传递给控制器的参数，如 ID 或其它各种变量

[URI 库]和[URL 辅助函数]这两个文档包含的函数让你更容易和 URI 数据打交道。另外，你的 URLS 可以使用 [URI 路由] 重定向，以获取更大的灵活性。

## 删除 index.php 文件

默认情况下：index.php 文件包含在你的 URLS 里：

	example.com/index.php/news/article/my_article

如果你的 Apache 服务器允许 `mod_rewrite `。你可以很容易的移除这个文件。下面是一个例子，使用"negative"方法将非指定内容重新定向：
	
	RewriteEngine On
	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteCond %{REQUEST_FILENAME} !-d
	RewriteRule ^(.*)$ index.php/$1 [L]

在上面的例子中，任何指向不存在的文件夹和文件的 HTTP 请求都被定向到 index.php。

注意：这些特定的规则不一定适合所有的服务器配置。

## 添加一个 URL 后缀

*config/config.php* 文件里，你可以指定一个后缀，这样 CodeIgniter 生成的所有 URL 都会添加上。例如 URL 如下：

	example.com/index.php/products/view/shoes

你可以选择增加一个后缀，比如 *.html,* ，这样 URL 就会变成：

	example.com/index.php/products/view/shoes.html

## 允许查询字符串

某些情况下，你可能会选择使用查询字符串 URL：

	index.php?c=products&m=view&id=345

CodeIgniter 可以有选择的支持这个功能，在 *application/config.php* 文件里可以打开，打开后，内容如下：

	$config['enable_query_strings'] = FALSE;
	$config['controller_trigger'] = 'c';
	$config['function_trigger'] = 'm';

如果将 "enable_query_strings" 改为 TRUE，这个功能就可用。通过`触发器`关键字来调用你的控制器和函数：

	index.php?c=controller&m=method

注意：如果你使用了查询字符串，你就需要创建自己的 URLs，而不是使用 URL 辅助函数（以及其他辅助生成 URLs 的函数，比如表单辅助函数），因为设计的时候他们是基于段的 URLs。


[URI 库]: uri.md
[URL 辅助函数]: url_helper.md
[URI 路由]: routing.md