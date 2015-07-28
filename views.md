# 视图

视图是一个简单的 web 页面，或者页面的部分，如页头，页尾，侧边栏等等。实际上，如果你需要这种树状类型，视图可以灵活的嵌入到其他视图（或者再嵌入其他视图）。

视图从不会被直接调用，必须通过控制器来调用。记住，在 MVC 架构中，控制器扮演了交通警察的角色，那么它就得负责取回一个特殊视图。如果你没有阅读过控制器文档，那么你需要先阅读一下。

使用之前创建的控制页面，并加入一个视图。

## 创建一个视图

使用你的文本编辑器，创建一个 blogview.php 文件，并输入：

	<html>
	<head>
		<title>My Blog</title>
	</head>
	<body>
		<h1>Welcome to my Blog!</h1>
	</body>
	</html>
	
并保存到 *application/views/* 文件夹。

## 加载一个视图

加载一个特点的视图文件，需要使用以下方法：

	$this->load->view('name');

上面的 name 就是你的视图文件。

注意： .php 后缀名不用特别指定，除非你使用了非 .php 的文件。

现在，打开之前你创建的 Blog.php，使用视图加载方法覆盖 echo 语句。

	<?php
	class Blog extends CI_Controller {

		public function index()
		{
			$this->load->view('blogview');
		}
	}

如果你使用之前的 URL 浏览网站，你将会看到你的新视图，URL 与下面类似：

	example.com/index.php/blog/

## 载入多个视图

CodeIgniter 将会智能的处理多个从控制器发起的 `$this->load->view()` 调用。如果超过一个调用，它们将会合并到一起。例如，你可能希望有一个页头视图，菜单视图，内容视图，页脚视图。它可能长成这样：

	<?php

	class Page extends CI_Controller {

		public function index()
		{
			$data['page_title'] = 'Your title';
			$this->load->view('header');
			$this->load->view('menu');
			$this->load->view('content', $data);
			$this->load->view('footer');
		}

	}

上述的列子中，我们使用动态添加数据方法，稍后你将会看到。

## 用子文件夹保存视图

如果你想让文件系统更有组织性，可以使用子文件夹来保存视图。当你载入视图的时候，必须带有子文件夹名。

	$this->load->view('directory_name/file_name');

## 添加动态数据到视图

控制器将数组或者对象作为第二个参数传递给视图加载方法。底下是使用数组的例子：

	$data = array(
		'title' => 'My Title',
		'heading' => 'My Heading',
		'message' => 'My Message'
	);

	$this->load->view('blogview', $data);

这个是使用对象作为参数：

	$data = new Someclass();
	$this->load->view('blogview', $data);

注意:如果你使用一个对象，类变量将会变为数组元素

让我们使用控制器文件试试，打开它并添加以下代码：

	<?php
	class Blog extends CI_Controller {

		public function index()
		{
			$data['title'] = "My Real Title";
			$data['heading'] = "My Real Heading";

			$this->load->view('blogview', $data);
		}
	}

现在打开你的视图文件，将文本改为和数组 key 对应的变量：

	<html>
	<head>
		<title><?php echo $title;?></title>
	</head>
	<body>
		<h1><?php echo $heading;?></h1>
	</body>
	</html>

接着使用之前的 URL 加载页面，你将看到变量已经替换。

## 创建循环

你传给视图的变量，不仅是一个简单变量。你也可以传递多维数组，他可以循环生成多行。例如，如果你从你的数据库里获取数据，它既是典型的多维数组。

这里有一个简单的例子，添加到你的控制器：

	<?php
	class Blog extends CI_Controller {

		public function index()
		{
			$data['todo_list'] = array('Clean House', 'Call Mom', 'Run Errands');

			$data['title'] = "My Real Title";
			$data['heading'] = "My Real Heading";

			$this->load->view('blogview', $data);
		}
	}

现在打开你的视图文件，并创建循环：

	<html>
	<head>
		<title><?php echo $title;?></title>
	</head>
	<body>
		<h1><?php echo $heading;?></h1>
	
		<h3>My Todo List</h3>

		<ul>
		<?php foreach ($todo_list as $item):?>
	
			<li><?php echo $item;?></li>
	
		<?php endforeach;?>
		</ul>

	</body>
	</html>

注意:你也许已经注意到了，上面的例子我们使用了 PHP 的替代语法。如果你不熟悉可以阅读 `alternative_php `文档.

## 返回视图

view 函数第三个可选参数可以改变函数的行为，让数据作为字符串返回而不是发送到浏览器。如果你想用其他方法处理数据，这会很有用。如果你将参数设置为 TRUE (boolean)，它将会返回数据。默认为 false，这样会发送给浏览器。如果你想要返回数据，记得将他赋值给变量。
There is a third **optional** parameter lets you change the behavior of
the method so that it returns data as a string rather than sending it
to your browser. This can be useful if you want to process the data in
some way. If you set the parameter to TRUE (boolean) it will return
data. The default behavior is false, which sends it to your browser.
Remember to assign it to a variable if you want the data returned::

	$string = $this->load->view('myfile', '', TRUE);
