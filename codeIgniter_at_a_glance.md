# 初探 CodeIgniter

## CodeIgniter 是一个应用框架

CodeIgniter 是 PHP 开发 web 应用的工具集。通过提供一套丰富常用库，简单的接口，和访问这些库的逻辑结构，它能让你从零开始开发的时候速度更快。CodeIgniter 可以让任务的代码量减少，这样你就可以将精力放在开发上。

## CodeIgniter 免费

CodeIgniter 是经过 MIT 开源许可授权的，只要你愿意就可以使用它。更多的信息参考`许可协议`。

## CodeIgniter 是轻量级的

真正的轻量级。系统核心仅需要非常小的库。它和其他需要很多资源的库明显不同。同时，它的附加库是运行时加载，根据你的进程的需求来定，所以核心库非常的轻且快。

## CodeIgniter 非常快

真的非常快。你可以试试找找比 CodeIgniter 性能更好的快。

## CodeIgniter 使用 M-V-C

CodeIgniter 使用了模型（Model）- 视图（View）- 控制器（Controllers）的方法，这样可以让逻辑层和表现层分离。这对工程的模板设计者非常有利，它能让代码量变少。更多 MVC 细节参考[模型（Model）- 视图（View）- 控制器（Controllers)]。

## CodeIgniter 生成干净的 URLs

CodeIgniter 生成 URLs 是干净的，并且对搜索引擎友好。不同于标准的“字符串查询”方法，CodeIgniter 使用了基于段（segment-based）的方法：

	example.com/news/article/345

注意: 默认情况下，index.php 文件包含在 URL，但是可以通过一个简单的 `.htaccess` 文件移除。

## CodeIgniter 功能强大

CodeIgniter 拥有全范围的类库，可以满足大多数网络开发任务的需求，包括： 读取数据库、发送电子邮件、数据确认、保存 session 、对图片的操作，以及支持 XML-RPC 数据传输等。

## CodeIgniter 可扩展

CodeIgniter 系统可以通过自定义的类库，辅助函数，类扩展，或系统钩子，简单的实现系统扩展。

## CodeIgniter 不需要模板引擎

虽然 CodeIgniter 自带了一个可选的模板解析器程序，但并不强制你使用。模板引擎与本地 PHP 性能不匹配，使用模板引擎我们要学习其语法，这最低限度只比学PHP基础要容易一点点。看看以下PHP 代码：

	<ul>
	<?php foreach ($addressbook as $name):?>
		<li><?=$name?></li>
	<?php endforeach; ?>
	</ul>

再来对比模板引擎所使用的伪代码：

	<ul>
	{foreach from=$addressbook item="name"}
		<li>{$name}</li>
	{/foreach}
	</ul>

模板引擎的例子更加干净一些，但是性能更差，因为它需要先转为 PHP 代码才能运行。因为我们的目标是**最佳性能**，所以我们不用模板引擎。

## CodeIgniter 已经彻底文档化

开发者一般喜欢写代码，而不喜欢写文档。我们当然也一样，不过既然文档和代码**一样重要**，我们就要完成它。我们的代码非常干净同时注释也非常优秀。

## CodeIgniter 的用户社区非常友好

我们的[社区](http://forum.codeigniter.com/)非常的活跃，大家参与的积极性非常高。



[模型（Model）- 视图（View）- 控制器（Controllers)]:model_view_controller.md
