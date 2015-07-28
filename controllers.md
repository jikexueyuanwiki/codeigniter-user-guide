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

## 将 URI 段传递给你的方法

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

注意：如果你使用了[URI 路由]特性，传递给你的方法的参数将会是重新路由过的对象。

## 定义一个默认控制器

当 URI 不存在，或者直接访问根目录的时候，CodeIgniter 将会载入默认控制器。打开 *application/config/routes.php* 文件，并输入以下内容，可以改变默认控制器：

	$route['default_controller'] = 'blog';

当 Blog 正是你想用使用的控制器类时。现在如果你加载 index.php，而没有指定任何 URI 段，默认情况下将会看到 "Hello World" 消息。

## 重新隐私调用方法

如上所述，URI 的第二个段通常会确定调用控制器里的哪个方法。CodeIgniter 允许你重写这个行为，通过使用 `_remap()` 方法。

	public function _remap()
	{
		// Some code here...
	}

注意: 如果你的控制器包含一个 _remap() 方法，它将会始终被调用，无论你的 URI 包含什么。它重写了正常的URI 决定调用哪个方法的行为，允许你来定义自己的路由规则。

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

Processing Output
=================

CodeIgniter has an output class that takes care of sending your final
rendered data to the web browser automatically. More information on this
can be found in the :doc:`Views <views>` and :doc:`Output Class
<../libraries/output>` pages. In some cases, however, you might want to
post-process the finalized data in some way and send it to the browser
yourself. CodeIgniter permits you to add a method named ``_output()``
to your controller that will receive the finalized output data.

.. important:: If your controller contains a method named ``_output()``,
	it will **always** be called by the output class instead of
	echoing the finalized data directly. The first parameter of the
	method will contain the finalized output.

Here is an example::

	public function _output($output)
	{
		echo $output;
	}

.. note::

	Please note that your ``_output()`` method will receive the
	data in its finalized state. Benchmark and memory usage data
	will be rendered, cache files written (if you have caching
	enabled), and headers will be sent (if you use that
	:doc:`feature <../libraries/output>`) before it is handed off
	to the ``_output()`` method.
	To have your controller's output cached properly, its
	``_output()`` method can use::

		if ($this->output->cache_expiration > 0)
		{
			$this->output->_write_cache($output);
		}

	If you are using this feature the page execution timer and
	memory usage stats might not be perfectly accurate since they
	will not take into account any further processing you do.
	For an alternate way to control output *before* any of the
	final processing is done, please see the available methods
	in the :doc:`Output Library <../libraries/output>`.

Private methods
===============

In some cases you may want certain methods hidden from public access.
In order to achieve this, simply declare the method as being private
or protected and it will not be served via a URL request. For example,
if you were to have a method like this::

	private function _utility()
	{
		// some code
	}

Trying to access it via the URL, like this, will not work::

	example.com/index.php/blog/_utility/

.. note:: Prefixing method names with an underscore will also prevent
	them from being called. This is a legacy feature that is left
	for backwards-compatibility.

Organizing Your Controllers into Sub-directories
================================================

If you are building a large application you might find it convenient to
organize your controllers into sub-directories. CodeIgniter permits you
to do this.

Simply create folders within your *application/controllers/* directory
and place your controller classes within them.

.. note:: When using this feature the first segment of your URI must
	specify the folder. For example, let's say you have a controller located
	here::

		application/controllers/products/Shoes.php

	To call the above controller your URI will look something like this::

		example.com/index.php/products/shoes/show/123

Each of your sub-directories may contain a default controller which will be
called if the URL contains only the sub-folder. Simply name your default
controller as specified in your *application/config/routes.php* file.

CodeIgniter also permits you to remap your URIs using its :doc:`URI
Routing <routing>` feature.

Class Constructors
==================

If you intend to use a constructor in any of your Controllers, you
**MUST** place the following line of code in it::

	parent::__construct();

The reason this line is necessary is because your local constructor will
be overriding the one in the parent controller class so we need to
manually call it.

Example::

	<?php
	class Blog extends CI_Controller {

		public function __construct()
		{
			parent::__construct();
			// Your own constructor code
		}
	}

Constructors are useful if you need to set some default values, or run a
default process when your class is instantiated. Constructors can't
return a value, but they can do some default work.

Reserved method names
=====================

Since your controller classes will extend the main application
controller you must be careful not to name your methods identically to
the ones used by that class, otherwise your local functions will
override them. See :doc:`Reserved Names <reserved_names>` for a full
list.

.. important:: You should also never have a method named identically
	to its class name. If you do, and there is no ``__construct()``
	method in the same class, then your e.g. ``Index::index()``
	method will be executed as a class constructor! This is a PHP4
	backwards-compatibility feature.

That's it!
==========

That, in a nutshell, is all there is to know about controllers.

[URI 路由]: routing.md