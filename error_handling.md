# 错误处理

CodeIgniter 允许你在应用中使用以下的函数建立错误报告。另外，它有一个错误日志类，允许错误和调试信息保存为文本文件。

注意:默认情况下，CodeIgniter 显示所有 PHP 错误。你可能想要在开发完成后改变这个行为。你可以在 index.php 顶部找到 error_reporting() 函数。即使禁用错误报告，发生错误时，错误日志也不回停止。

CodeIgniter 和其他系统不太一样，错误报告函数是一个简单的程序接口，可以在整个应用程序里使用。不用考虑类或者是函数的范围，这种办法可以直接触发错误通知。

无论系统核心何时调用 `exit()`， CodeIgniter 也会返回一个状态码。退出状态码独立于 HTTP 状态码，这个服务监视其他应用程序是否成功的执行完成，如果没有成功，是什么原因导致的。这些值定义在 *application/config/constants.php*。退出状态码在 CLI 设置中很有用，返回的状态码能让服务器软件追踪脚本代码，让你的程序更健壮。

以下函数能让你产生错误：

## show\_error($message, $status_code, $heading = 'An Error Was Encountered')

* 参数 混合$message: 错误消息
* 参数 整数	$status_code: HTTP 状态码
* 参数 字符串	$heading: 错误页面头
* 返回类型:	void

这个函数将会使用以下模板来显示错误信息：

		application/views/errors/html/error_general.php

或

		application/views/errors/cli/error_general.php


可选参数 `$status_code` 确定将会发生什么 HTTP 状态码到错误。如 `$status_code` 少于 100，HTTP 状态码将会设置为 500，退出状态码将会设置为：`$status_code + EXIT__AUTO_MIN`。如果这个值大于 `EXIT__AUTO_MAX`，或者如果 `$status_code` 为大于等于 100 的值，退出码将会设置为 `EXIT_ERROR`。更多细节参考 *application/config/constants.php*。

## show\_404($page = '', $log_error = TRUE)

* 参数 字符串	$page: URI 字符串
* 参数	bool	$log_error: 是否将错误写入日志
* 返回类型:	void

这个函数将会用下列的模板显示 404 错误信息：

		application/views/errors/html/error_404.php

或

		application/views/errors/cli/error_404.php

这个函数希望传入的字符串是没有找到的文件路径。退出状态码将会设置为 `EXIT_UNKNOWN_FILE`。注意如没有找到控制器 CodeIgniter 将会自动的显示 404 消息。CodeIgniter 自动将任何 `show_404()` 调用写入日志。将第二个参数设置为 FALSE，将会跳过日志。

## log\_message($level, $message, $php_error = FALSE)

* 参数 字符串	$level: 日志级别 'error', 'debug' 或 'info'
* 参数 字符串	$message: 写入日志的消息
* 参数	bool	$php_error: 是否将原生的 PHP 错误写入日志
* 返回类型:	void

这个函数能让你将消息写入到日志。你必须提供第一个参数的三个日志级别之一，它表示写的日志是哪种消息，第二个参数就是要写入的消息。例如：

		if ($some_var == '')
		{
			log_message('error', 'Some variable did not contain a value.');
		}
		else
		{
			log_message('debug', 'Some variable was correctly set');
		}

		log_message('info', 'The purpose of some variable is to provide some value.');

有三种消息类型:

* 错误消息. 比如 PHP 错误或用户错误
* 调试信息. 辅助调试的信息。例如，如果一个类已经初始化，你可以记录这个信息。
* 普通消息. 这是最低级别的消息，只是简单了提供了一些运行时的信息

注意:  想要写日志，需要确保 *logs/* 文件夹可写。另外，必须设置 *application/config/config.php* 里的 "threshold"。例如，你可以只写错误日志，而其他两种日志不写。如果你设置为 0，将禁用日志。
