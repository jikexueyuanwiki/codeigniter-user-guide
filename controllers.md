# 控制器

控制器是应用程序的核心，因为它决定了如何处理 HTTP 请求。

## 什么是控制器

**控制器是一个简单的类文件，它的命名方式能和 URI 相关联。**

看看这个 URI：

	example.com/index.php/blog/

在上述的例子中，CodeIgniter 尝试寻找一个名为 Blog.php 的控制器并加载。

**当一个控制器名字和 URI 第一个部分相符合时，将会被加载**

## 让我试试: Hello World!

让我创建一个简单的控制器以便观察他是如何工作的。使用你的文本编辑器，创建一个 Blog.php 的文件，并加入以下代码：

	<?php
	class Blog extends CI_Controller {

		public function index()
		{
			echo 'Hello World!';
		}
	}

保存到 *application/controllers/* 文件夹.

注意: 文件名必须是'Blog.php'，首字母 'B' 大写.

现在，可以使用和下面类似 URL 访问你的站点：

	example.com/index.php/blog/

如果一切正确，你将会看到：

	Hello World!

注意: 类名必须以大写字母开头

这个是有效的:

	<?php
	class Blog extends CI_Controller {

	}
	
这个是无效的：

	<?php
	class blog extends CI_Controller {

	}

同时要保证你的控制器继承自父控制器，它能继承所有的方法。

## 方法

上面的列子，方法名 `index()`。如果第二部分为空，将会载入 "index" 方法。第二种显示 "Hello World" 消息的方法如下：

	example.com/index.php/blog/index/

**URI 的第二段决定了将会调用控制器里的哪个方法。**

让我们试试添加一个新方法到你的控制器：

	<?php
	class Blog extends CI_Controller {

		public function index()
		{
			echo 'Hello World!';
		}

		public function comments()
		{
			echo 'Look at this!';
		}
	}

现在用下面的 URL 试着调用第二个方法：

	example.com/index.php/blog/comments/

你将会看到新的消息。

## 将 URI 段作为参数传递给你的方法

如果你的 URI 的段落超过 2 个，他们将会作为参数传递。

例如，我们有一个 URI 如下：

	example.com/index.php/products/shoes/sandals/123

你的方法将会受到参数 3 和 4（"sandals" 和 "123"）：

	<?php
	class Products extends CI_Controller {

		public function shoes($sandals, $id)
		{
			echo $sandals;
			echo $id;
		}
	}

注意：如果你使用了 URI 路由特性，传递给你的方法的参数将会是重新路由过的对象。

## 定义一个默认控制器

当 URI 不存在，或者直接访问根目录的时候，CodeIgniter 将会载入默认控制器。打开 *application/config/routes.php* 文件，并输入以下内容，可以改变默认控制器：

	$route['default_controller'] = 'blog';

当 Blog 正是你想用使用的控制器类时。现在如果你加载 index.php，而没有指定任何 URI 段，默认情况下将会看到 "Hello World" 消息。

## 重新映射调用方法

如上所述，URI 的第二个段通常会确定调用控制器里的哪个方法。CodeIgniter 允许你重写这个行为，通过使用 `_remap()` 方法。

	public function _remap()
	{
		// Some code here...
	}

注意: 如果你的控制器包含一个 _remap() 方法，它将会始终被调用，无论你的 URI 包含什么。它重写了正常的 URI 决定调用哪个方法的行为，允许你来定义自己的路由规则。

被重新定义的方法调用方式（一般是 URI 中的第二段）将作为一个参数传递给 `_remap()` ：


	public function _remap($method)
	{
		if ($method === 'some_method')
		{
			$this->$method();
		}
		else
		{
			$this->default_method();
		}
	}

任何在该方法名称之后的附加段都会被视为 `_remap()` 的第二个参数（可选）。这个可选的数组参数可以与 PHP 的 `call_user_func_array` 联用，模拟 CodeIgniter 的默认行为。

例如:

	public function _remap($method, $params = array())
	{
		$method = 'process_'.$method;
		if (method_exists($this, $method))
		{
			return call_user_func_array(array($this, $method), $params);
		}
		show_404();
	}

## 处理输出

CodeIgniter 拥有一个输出类用来确保你修改的数据会自动被传递给浏览器。关于这个的更多信息可以在视图和输出类里找到。有些时候，你可能想要自己发布修改一些最终的数据或是自己把它传递给浏览器。CodeIgniter 允许你给你的控制器增加一个名为 _output() 的方法来接收最终的数据。

注意：如果你的控制器包含一个 _output() 方法，那么它将总是被调用，而不是直接输出最终的数据。这个方法类似于OO里的析构函数，不管你调用任何方法这个方法总是会被执行。例如：


	public function _output($output)
	{
		echo $output;
	}

注意：你的 _output() 将接收最终的数据。 Benchmark 和内存的使用率数据将被渲染，缓存文件会被写入（如果已启用缓存），并且 HTTP 头也将被发送（如果您使用该功能），然后交给 _output() 函数。

为了让你的控制器输出缓存正确, 它的 _output() 函数可以这样来写:


		if ($this->output->cache_expiration > 0)
		{
			$this->output->_write_cache($output);
		}


如果您正在使用页面执行时间和内存使用统计的功能，这可能不完全准确，因为他们不会考虑到你所做的任何进一步的动作。请在输出类参用可用的方法，来控制输出以使其在任何最终进程完成之前执行。

## 私有方法

在某些情况下，你可能想要隐藏一些方法使之无法对外查阅。将方法私有化很简单，只要在方法名字前面加一个下划线（“_”）做前缀就无法通过 URL 访问到了。例如，如果你有一个像这样的方法：

	private function _utility()
	{
		// some code
	}

那么，通过下面这样的 URL 进行访问是无法访问到的：

	example.com/index.php/blog/_utility/

注意:前缀方法名称使用下划线，是为了避免被调用。这是原本就有的功能，目的是向后兼容。 

## 如何将控制器放入子文件夹中

如果你在建立一个大型的应用程序，你会发现 CodeIgniter 可以很方便的将控制器放到一些子文件夹中。

只要在 application/controllers 目录下创建文件夹并放入你的控制器就可以了。

注意：如果你要使用某个子文件夹下的功能，就要保证 URI 的第一个片段是用于描述这个文件夹的。例如说你有一个控制器在这里：

		application/controllers/products/Shoes.php

调用这个控制器的时候你的 URI 要这么写

		example.com/index.php/products/shoes/show/123

你的每个子文件夹中需要包含一个默认的控制器，这样如果 URI 中只有子文件夹而没有具体功能的时候它将被调用。只要将你作为默认的控制器名称在 application/config/routes.php 文件中指定就可以了。

CodeIgniter 也允许你使用 URI 路由 功能来重新定向 URI。

## 构造函数

如果要在你的任意控制器中使用构造函数的话，那么必须在里面加入下面这行代码：

	parent::__construct();

这行代码的必要性在于，你此处的构造函数会覆盖掉这个父控制器类中的构造函数，所以我们要手动调用它。例如:

	<?php
	class Blog extends CI_Controller {

		public function __construct()
		{
			parent::__construct();
			// Your own constructor code
		}
	}

如果你需要设定某些默认的值或是在实例化类的时候运行一个默认的程序，那么构造函数在这方面就非常有用了。

构造函数并不能返回值，但是可以用来设置一些默认的功能。

## 保留的方法名称

因为你添加的控制器类继承了主要的应用程序控制器，所以你要小心你的方法名不要和那个类中的方法名一样了，否则你的方法会覆盖原有的。详细信息请查看保留字部分

注意: 你不要让方法名和类名相同。如果你这么做，将不会有构造函数，然后 `Index::index()` 方法将会作为类的构造函数执行。这是 PHP 4 向后兼容的特性。

## 就这样

好，以上就是有关控制器的所有内容了。