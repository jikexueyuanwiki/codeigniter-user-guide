# 处理多环境

开发者通常希望在开发环境和生产环境有不同的行为。例如错误信息在开发中有用，而在项目上线后者可能会造成一些安全问题。

## ENVIRONMENT 常量

默认情况下，CodeIgniter 把环境常量 `$_SERVER['CI_ENV']` 设置为 'development'，在 index.php 的顶部，你会看到：

	define('ENVIRONMENT', isset($_SERVER['CI_ENV']) ? $_SERVER['CI_ENV'] : 'development');

这个服务器变量可在 .htaccess 文件中设置，或者 Apache 使用 `SetEnv`设置。这个方法对于 nginx 或其他方法有效，或者你可以整个移除这个逻辑，并根据服务器 IP 设置常量。

除了影响基本框架的行为（参见下个章节），你可以在你的开发环境中使用这个常量，以便区别于不同的环境。

## 	对默认框架行为的影响

CodeIgniter 系统中有哪些地方使用了 ENVIRONMENT 常量。这个部分描述了默认系统框架行为如何受到影响。

### 错误报告

将环境变量设置为 'development' ，会让 PHP 错误都输出到浏览器。相反，如果设置为 'production'，将会禁用错误输出。在产品中禁止错误输出是一个不错的安全策略。

### 配置文件

可选的，你可以让 CodeIgniter 加载特定的环境配置文件。这可能会对管理多环境使用不同 API 密钥这样的事情很有用。这在文档配置类“环境”一节有详细的说明。