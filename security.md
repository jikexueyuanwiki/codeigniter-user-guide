# 安全

本章描述了 web 安全的“最佳实践”，详细说明了 CodeIgniter 的内部安全特性。

## URI 安全

CodeIgniter 严格限制 URI 中所包含的字符，让你设计的程序减少被恶意数据入侵的可能。URI 一般只包含以下内容：

-  字母和数字 (仅拉丁字符)
-  波浪号: ~
-  百分号: %
-  点: .
-  冒号: :
-  下划线: \_
-  减号: -
-  空格

## Register_globals

系统初始化的时候，所有的全局变量都被 unset，除了那些 `$_GET`, `$_POST`, `$_REQUEST` and `$_COOKIE`数组中得内容。实际上 unsetting 实例程序与register_globals = off 作用相同。

## display_errors

在生产环境中，通常都通过设置 *display_errors* 值为 0 ，来禁用 PHP 的错误报告。它能禁用 PHP 错误输出，它可能包含敏感信息。

设置 index.php 里的 **ENVIRONMENT** 常量为 **'production'**，将会关闭这些错误。在开发模式中，还是尽量用 'development'。更多开发环境和生产环境的细节参考“处理环境”文档。

## magic\_quotes\_runtime

在系统初始化时， magic\_quote\_runtime 指令被关闭，以便在数据库检索数据时不必去掉反斜线。

## 最佳实践

在接收任何数据到你的程序前，不管是表单提交的 POST 数据，COOKIE 数据，URI 数据、XML-RPC 数据、还是 SERVER 数组中的数据，我们都推荐你实践下面的三个步骤：

* 过滤不良数据.
* 验证数据以确保正确的类型, 长度, 大小等. (有时这一步也可取代第一步)
* 在提交数据到数据库前转码

CodeIgniter 提供了以下函数和提示来帮助你完成这个过程：

### XSS 过滤

CodeIgniter 带有一个跨站脚本过滤器，这个过滤器会查找那些常用手段嵌入到你数据中恶意的Javascript，或其它一些试图欺骗 cookie 做其它恶意事情的代码。 XSS Filter 的详细描述在参见安全性文档。

注意: XSS 过滤仅在输出时起作用。过滤输入数据可能会修改原始数据，这并不是我们想要的，比如从密码中剥离特殊字符，我们宁愿减少安全性而不是替换它。

## CSRF 保护

CSRF 表示跨站伪造请求（Cross-Site Request Forgery）,攻击者欺骗受害人到一个他自己不知道的地方提交请求。 

CodeIgniter 提供 CSRF 保护立即可用，它会自动触发每个非 Get HTTP 请求，但是也要求你以某种形式创建自己的提交表单。详情参见文档“安全库”。

## 密码处理

在应用中，密码处理是非常关键的。

不幸的是，很多开发者并不知道如何处理，很多网页都过时了，或者充满错误。

我们将会给你的可以做和不可以做的列表来帮助你：

-  **不要**将密码存储为文本格式，必须**哈希**密码。

-  **不要**使用 Base64 或类似的编码来存储密码。这和以文本格式存储密码一样。一定要**哈希**，而不是编码。编码和加密是两种处理方法。密码必须是本人才能知道，因此只能这么做。哈希算法是不可逆的。

-  **不要**使用弱哈希算法，例如 MD5 或者 SHA1。这些算法已经过时，并且是有缺陷的，所以不是好的密码哈希的算法。同时，**不要**发明你自己的哈希算法。必须使用强哈希算法，比如 BCrypt，它是 PHP 自己的哈希函数。请使用他们，即使你的 PHP 版本不是 5.5+，CodeIgniter 为你提供的版本最低是 5.3.7（如果你的版本达不到要求，请升级）。如果你不能升级到新版本，请使用 [hash_pbkdf()](http://php.net/hash_pbkdf2) 函数，它也提供了兼容算法。

-  **不要**以明文的形式发送密码！即使对于密码拥有者，如果使用了“忘记密码”功能，重新生成一个随机的一次性密码，将它发送给用户。

-  **不要**给密码设置不必的限制。如果你使用的哈希算法不是 BCrypt（它限制长度为 72 位），你必须将密码设置一个相对多一些位数，以抵制 Dos 攻击，比如 1024 字节。不必限制密码必须是字符或者数字，它可以包含特殊字符。

## 验证输入数据

CodeIgniter 中得“表单验证库”可以帮助你验证，过滤，并准备数据。

即使这些东西不起作用，你也要验证并净化所有输入数据。例如，如果你希望一个输入必须是数字，你可以使用 `is_numeric()` 或 `ctype_digit()` 来检查。

需要注意，这个检查不仅对于 `$_POST` 和 `$_GET` 变量，同时也要检查 cookies，user-agent 字符串和那些并不是你的代码创建的基础数据。


## 插入数据库前转义所有数据

转义前的数据不要插入到数据库，更多细节参考文档**数据库查询**。

### 隐藏你的文件

另一个安全的实践是，在你的服务器的根目录里仅保留 *index.php* 和 "资源"（比如 .js css 和图片文件）（一般命名为 "htdocs/"）。这些文件都是访问网站所需的文件。

让你的访问者可以看到任何东西，他们可能会访问到敏感数据，执行脚本等。

如果你不禁止这么做，你可以尝试使用 .htaccess 文件来限定访问这些资源。

CodeIgniter 在所有的文件将会拥有一个 index.html 文件，可以试着隐藏一部分，不过你要注意，这些都不能阻止高级攻击者。
