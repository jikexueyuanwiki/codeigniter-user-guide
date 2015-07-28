# 静态页面

**注意:** 这个文档假设在你的开发环境中已经下载了 CodeIgniter 和文档。

首先，你需要创建一个能处理静态页面的控制器类。控制器类可以帮助代理工作。它是 web 应用程序的粘合剂。

例如, 假设调用了以下 URL 请求:

	http://example.com/news/latest/10

我们可以假设有一个控制器 "news"。调用此类下的 “latest” 方法，获取最新的 10 条内容，并显示到页面上。基于 MVC 架构，通常我们可以看到 URL 模式如下：

	http://example.com/[controller-class]/[controller-method]/[arguments]

实际上 URL 语法可能会更复杂，这个模式也会发生变化。但是到目前为止，我们任然需要了解这个模式。

创建一个文件位于 `application/controllers/Pages.php`,代码如下：

	<?php 
	class Pages extends CI_Controller { 

		public function view($page = 'home') 
		{
	        }
	}

你已经创建了一个 `pages` 类，包含一个名卫 `view` 的方法，参数卫 `$page`.`page` 类继承自 `CI_Controller` 类。这个新的 pages 类可以继承 `CI_Controller` (system/core/Controller.php) 类里面定义的方法和变量。

这个控制器将会成为 **web 应用煤气请求的中心**。在 CodeIgniter 技术讨论中，它被称为超级对象。和任何 php 类一样，可以通过 `$this` 来调用这个控制器。通过 `$this`，可以加载库，视图，和常用的框架。

现在你已经有了第一个方法，可以开始创建基础页面模板了。我们将会创建两个视图（页面模板），页头和页尾。

创建头文件 *application/views/templates/header.php*，添加以下代码：

	<html>
		<head>
			<title>CodeIgniter Tutorial</title>
		</head>
		<body>

			<h1><?php echo $title; ?></h1>

头文件包含加载主视图前的 HTML 基础代码，。同时也会输出稍后我们会在控制器里定义的 `$title` 变量。接着，创建页尾 *application/views/templates/footer.php* 包含以下代码。

			<em>&copy; 2015</em>
		</body>
	</html>

## 给控制器添加逻辑代码

之前你创建了一个包含 `view()` 方法的控制器。这个方法包含一个参数，它是将要加载的页面。静态页面路径： *application/views/pages/* 。

这个文件夹里，创建两个文件 *home.php* 和 *about.php*。在这两个文件里，输入一些文字，任何文字都可以，并保存。如果你喜欢不寻常的内容，可以试试 "Hello World"。

为了加载这些页面，你需要检查请求页面是否真的存在：

	public function view($page = 'home')
	{
	        if ( ! file_exists(APPPATH.'/views/pages/'.$page.'.php'))
		{
			// Whoops, we don't have a page for that!
			show_404();
		}

		$data['title'] = ucfirst($page); // Capitalize the first letter

		$this->load->view('templates/header', $data);
		$this->load->view('pages/'.$page, $data);
		$this->load->view('templates/footer', $data);
	}

现在，如果页面存在，将会被加载并显示给用户，包含页头，页尾。如果页面不存在，将会显示 `404` 页面。

第一行代码检查页面文字是否存在。PHP 代码 `file_exists()` 检查文件是否存在。`show_404()` 是 CodeIgniter 内置函数指向默认的错误页面。

在头文件模板中，`$title` 变量用来初始化页面标题。标题的内容在这个方法中定义，不过我们并没有将值直接赋值给变量，而是把它塞入了 `$data` 数组。

最后需要按显示顺序加载视图。`view()` 方法的第二个参数用来给视图传值。`$data` 数组中的每个值都被定义成与它关键字相同的一个变量，如控制器中 `$data['title']` 的值就等同于视图中变量 `$title`。

## 路由

控制器现在可以工作了！在浏览器中输入 `[your-site-url]index.php/pages/view` 可以看到你的页面，输入 `index.php/pages/view/about` 可以看到你的关于页面，它也包含页头和页尾。

使用自定义的路由规则，你可以映射任意 URI 到任意的控制器和方法，这样就可以摆脱常用的转换了：`http://example.com/[controller-class]/[controller-method]/[arguments]`

让我们来试试。打开路由文件 *application/config/routes.php*，并且添加以下代码。删除设置 `$route` 数组的相关代码：

	$route['default_controller'] = 'pages/view';
	$route['(:any)'] = 'pages/view/$1';

CodeIgniter 从头到尾读取路由规则，并且路由到即一个匹配规则上。每一个规则都是一个正则表达式（左侧）映射到一个由斜线分隔的控制器和方法名（右侧）。当有请求时，CodeIgniter 查找第一个匹配规则，并且调用合适的控制器和方法，可能还会带参数。

更多和 URI 路由相关的信息可以查看[路由文档].

上面的代码第二行是指利用通配符 (:any) 可以使任何请求都能匹配到 `$routes` 数组，并且通过参数传递给pages 类的 view() 方法。

现在访问：`index.php/about`。看看是否已经能正确地显示页面了呢？真帅！

[路由文档]: routing.md
