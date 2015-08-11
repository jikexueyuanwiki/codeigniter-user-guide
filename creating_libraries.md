# 创建类库

当我们使用术语**类库**时，我们一般指的是位于 libraries 文件夹中的类，并在这个文档的“类库参考”中介绍过。在这里，我们讨论一下如何在 application/libraries 文件里创建自己的类库，并使你的本地资源和全局框架资源保持独立。

作为一个额外的功能，如果你只是简单给存在的库添加一些函数，CodeIgniter 允许你的库扩展自原始类。甚至你可以在 *application/libraries* 文件夹里，放同名的类库文件来替换原始库。

总之:

-  你可以创建全系的类库
-  你可以扩展原始库
-  你可以替换原始库

下面的内容将详细介绍这 3 点。

注意: 数据库类不能被扩展或替换，其他的都可以被扩展或替换。

## 存储

你的类库必须放在  *application/libraries* 文件夹里，因为 CodeIgniter 将会在他们初始化的时候到这里找。

## 命名约定

-  文件首字母大写，列如: Myclass.php
-  类声明首字母大写，例如: class Myclass
-  类名和文件名必须匹配

## 类文件

所有的类必须有基础原型：

	<?php
	defined('BASEPATH') OR exit('No direct script access allowed'); 

	class Someclass {

		public function some_method()
		{
		}
	}

注意:我们这里用 Someclass 为例。

## 使用你自己的类

在控制器方法中，你可以使用以下的标准方法来初始化你的类：

	$this->load->library('someclass');

其中 *someclass* 是文件名，没有 ".php" 文件扩展。文件名不区分大小写。

加载后，你可以用小写名来访问你的类：

	$this->someclass->some_method();  // Object instances will always be lower case

## 初始化类的时候传递参数

在类库加载方法中，你可以动态的用数组传递给第二个参数，它将会传给构造函数：

	$params = array('type' => 'large', 'color' => 'red');

	$this->load->library('someclass', $params);

如果你使用这个特性，就必须给你的构造函数加上参数：

	<?php defined('BASEPATH') OR exit('No direct script access allowed');

	class Someclass {

		public function __construct($params)
		{
			// Do something with $params
		}
	}

你也可以传递存储于配置文件中的参数。只需创建一个配置文件，他的文件名和类名一致，并存储在 *application/config/* 文件夹里。注意，如果你按如上所述的方法传递参数，配置文件选项将不起作用。

## 在你的库里初始化 CodeIgniter 资源

要访问 CodeIgniter 的原生资源，可以在你的库里使用 `get_instance()` 方法。这个方法返回 CodeIgniter 父对象。

通常来说在你的控制器类里，你可以使用 `$this` 调用任何 CodeIgniter 方法。

	$this->load->helper('url');
	$this->load->library('session');
	$this->config->item('base_url');
	// etc.

``$this``, 只直接作用于你的控制器，数据模型，或视图中。如果你想在自定义类中使用 CodeIgniter 类，可以这么做：

首先，将 CodeIgniter 对象赋值给变量：

	$CI =& get_instance();

一旦你将对象赋值给变量，你就可以使用那个变量名，取代 `$this`：

	$CI =& get_instance();

	$CI->load->helper('url');
	$CI->load->library('session');
	$CI->config->item('base_url');
	// etc.

注意: 你可能已经注意到 `get_instance()` 函数通过引用传递：
	
		$CI =& get_instance();

这个非常重要。通过引用赋值运行你使用原始的 CodeIgniter 对象，而不是复制一个。

然而，因为类库是一个类，你最好使用 OOP 原则。因此，为了在所有的类方法中更好的使用 CodeIgniter 超级对象，你应该将它赋值给一个属性。

	class Example_library {

		protected $CI;

		// We'll use a constructor, as you can't directly call a function
		// from a property definition.
		public function __construct()
		{
			// Assign the CodeIgniter super-object
			$this->CI =& get_instance();
		}

		public function foo()
		{
			$this->CI->load->helper('url');
			redirect();
		}

		public function bar()
		{
			echo $this->CI->config->item('base_url');
		}

	}

## 使用你的版本替换原生库

简单的将你自己的类命名为原始类一样的名字，就能让 CodeIgniter 使用这个新的类。要使用这个特性，你的文件和类名必须和原生类完全一致。例如，为了替换原生的 Email 库，你可以创建一个文件 *application/libraries/Email.php*，并且声明你的类：

	class CI_Email {
	
	}

注意，多数原生类都以 `CI\_` 作为前缀。

可以用标准方法加载你的库：

	$this->load->library('email');

注意: 原生数据库类不能被你的自定义类替换。

## 扩展原生类库

如果你仅仅需要给原生库添加一些函数，而不是完全替换它。你最好简单的扩展一下原生库。扩展一个类就像在一个类中添加一些例外：

-  扩展的类必须声明是由父类扩展而来
-  你的新类名和类文件，必须以 `MY\_` 作为前缀（这个是可配置的，参见下面说明）。

例如，要扩展原生的 Email 类，你需要创建的文件名为 *application/libraries/MY_Email.php*，并且需要声明类：

	class MY_Email extends CI_Email {

	}

如果你需要在类里使用一个构造函数，就需要显式的继承父类的构造函数：

	class MY_Email extends CI_Email {

		public function __construct($config = array())
		{
			parent::__construct($config);
		}

	}

注意: 并不是所有的类库都在构造函数中拥有相同的参数。在扩展前先看看原生类库是如何实现的

## 载入你的子类

要加载子类，你需要使用标准的字符名。不要使用前缀。例如，要载入上文说过的 email 扩展子类，你应该这么写：

	$this->load->library('email');

一旦加载，就能像一般类一样使用他们。Email 类中得所有函数都能调用：

	$this->email->some_method();

## 设置自定义前缀

要设置你的自定义前缀，打开 *application/config/config.php*  文件，找到：

	$config['subclass_prefix'] = 'MY_';

请注意，所有的原生 CodeIgniter 类库前缀都是 `CI\_`，所以不要使用这个前缀。