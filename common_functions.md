# 公共函数

CodeIgniter 使用了一些全局定义的函数来完成某些操作，你在任何情况下都可以使用，而不需要加载库或辅助函数。

## is_php($version)

* 参数 字符串 $version: 版本号
* 返回：如果运行中得 PHP 版本大于等于 version，返回 TRUE，否则返回 FALSE
* 返回类型：bool

检测 PHP 版本是否大于参数的版本号。例如:

		if (is_php('5.3'))
		{
			$str = quoted_printable_encode($str);
		}

如果安装的 PHP 版本号大于等于参数 version，返回 TRUE，如果小于参数 version，则返回 FALSE。

## is\_really\_writable($file)

* 参数 字符串 $file: 文件路径
* 返回：如果路径可写，返回 TRUE，否则返回 FALSE
* 返回类型：bool

在Windows平台，实际上 `is_writable()` 函数在没有文件写权限时也返回 TRUE。那是因为，只有文件是只读属性时，操作系统才向 PHP 报告为 FALSE。这个函数依靠对文件的先行写入来判断是否真的具有写权限。通常情况下，只有在这个信息不可靠的平台上才推荐使用。例如:

		if (is_really_writable('file.txt'))
		{
			echo "I could write to this if I wanted to";
		}
		else
		{
			echo "File is not writable";
		}

## config_item($key)

* 参数 字符串 $key: 配置选项键值
* 返回：配置值或者 NULL
* 返回类型：混合

尽管使用 config_item() 函数能够取得单个配置信息，但是 `配置库` 是访问这些信息的优选方式。更多信息请见类库参考。

## show\_error($message, $status_code[, $heading = 'An Error Was Encountered'])

* 参数 混合 $message: 错误信息
* 参数 整数 $status_code: HTTP 响应状态码
* 参数 字符串 $heading: 错误页面头
* 返回类型：void

这个函数调用 `CI_Exception::show_error()`。更多细节参考文档“错误处理”文档。

## show\_404([$page = ''[, $log_error = TRUE]])

* 参数 字符串 $page: URI 字符串
* 参数 bool $log_error: 是否将错误写入日志
* 返回类型：void

这个函数调用 `CI_Exception::show_404()`。更多细节参考文档“错误处理”文档。

## log_message($level, $message)

* 参数 字符串 $level: 日志级别：'error', 'debug' 或 'info'
* 参数 字符串 $message: 写入日志的消息
* 返回类型：void

这是 `CI_Log::write_log()` 函数的别名。更多细节参考文档“错误处理”文档。

## set\_status_header($code[, $text = ''])

* 参数 整数 $code: HTTP 响应状态码
* 参数 字符串 $text: 状态码伴随的消息
* 返回类型：void	

允许你手工设置服务器状态头，例如：

		set_status_header(401);
		// Sets the header as:  Unauthorized

## remove\_invisible\_characters($str[, $url_encoded = TRUE])

* 参数 字符串 $str: 输入字符串
* 参数 bool $url_encoded: 是否移除 URL-encoded 字符串
* 返回：过滤后的字符串
* 返回类型：string		

这个函数不允许在 ASCII 字符间插入 NULL 字符，比如 Java\\0script。例如：

		remove_invisible_characters('Java\\0script');
		// Returns: 'Javascript'

## html_escape($var)

* 参数 混合 $var: 需要转义的字符串或数组
* 返回：转义过的 HTML 字符串
* 返回类型：混合		

这是原生 `htmlspecialchars()` 函数的别名，它能接受字符串数组。有助于防止跨站脚本攻击（XSS）。

## get_mimes()

* 返回：和文件类型相关的数组
* 返回类型：数组	

这个函数返回 *application/config/mimes.php* 里的 MIMEs 数组的引用。

## is_https()

* 返回：如果当前使用 HTTP-over-SSL，返回 TRUE，否则返回 FALSE
* 返回类型：bool	

如果使用安全连接（HTTPS），返回 TRUE，否则返回 FALSE

## is_cli()

* 返回：如果当前运行环境是 CLI，返回 TRUE，否则返回 FALSE。
* 返回类型：bool	

如果应用程序运行在命令行，返回 TRUE，否则返回 FALSE。这个函数同时检查 `PHP_SAPI` 是否是 'cli'，或者如果 `STDIN` 常量已经定义过。

## function\_usable($function_name)

* 参数 字符串 $function_name：函数名
* 返回：如果函数可用返回 TRUE，否则返回 FALSE
* 返回类型：bool	

如果一个函数存在并且可用，返回 TRUE，否则返回 FALSE。 这个使用 `function_exists()` 函数检查，并且如果已经加载 `Suhosin extension` ，检查是否它有没有关闭这个函数的检查功能。

如果你想检查诸如 `eval()` 和 `exec()` 函数是否可用，这个函数非常有用。这个函数也非常的危险，因此在高度严格的安全策略服务器可能被禁用。

注意：此功能被引入，是为了 Suhosin 终止脚本运行，但事实证明这是一个错误。这个错误修复了有一段时间了（版本 0.9.34），但是很遗憾它还没发布。