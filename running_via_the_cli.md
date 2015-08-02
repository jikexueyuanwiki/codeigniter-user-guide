# 通过 CLI 执行 CodeIgniter

除了通过浏览器 URL 调用控制器外，也可以通过命令行接口（CLI）调用。

## 什么是 CLI？

命令行接口是一种基于文本的和计算机交互的方式。更多信息参考[维基百科文章](http://en.wikipedia.org/wiki/Command-line_interface)。

## 为什么使用命令行运行？

这里有很多原因通过命令行运行 CodeIgniter，不过总是被忽略。

-  使用 cron 定时运行任务而不需要使用 wget 或 curl
-  通过检查 $this->input->is\_cli\_request() 让你的 cron 任务无法通过网址访问到
-  让交互式任务可以做设置权限、清空缓存、执行备份等操
-  与其他语言进行集成。比如一个 C++ 脚本可以调用一条指令来运行你模型中的代码!

## 让我们试试: Hello World!

让我们创建一个简单的控制，这样你可以看到他时如工作的。使用你的文本编辑器，创建一个文件 Tools.php，加入以下代码：

	<?php
	class Tools extends CI_Controller {

		public function message($to = 'World')
		{
			echo "Hello {$to}!".PHP_EOL;
		}
	}

将文件保存到 *application/controllers/* 文件夹。

现在正常情况下你可以通过你的网站的 URL 来访问它：

	example.com/index.php/tools/message/to

除此之外，我们也可以在 mac/linux 中打开终端，或在 Windows 中运行 “cmd”，并进入我们的 CodeIgniter 项目的目录。

	$ cd /path/to/project;
	$ php index.php tools message

如果你做的没问题，将会看到 *Hello World!*。

	$ php index.php tools message "John Smith"

这里我们像使用 URL 参数一样给它传递了一个参数。“John Smith” 参数被传入，并且输出也变成：

	Hello John Smith!

## 就是这样!

简单的来说，这就是全部你需要知道的有关命令行中使用控制器的事情了。记住这只是一个普通的控制器，所以路由和 _remap 也照样工作。