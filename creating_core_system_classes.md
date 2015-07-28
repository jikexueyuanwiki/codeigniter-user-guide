# 创建核心系统类

每次 CodeIgniter 运行时，都有很多基础类作为核心架构的一部分被初始化。你也可以使用你自己的类替换核心系统类，或者扩展核心系统类。

**多数用户没有这个需求，但对于那些想较大幅度的改变CodeIgniter的人来说,我们依然提供了替换和扩展核心系统类的选择**

注意: 改变核心系统类会有很大的影响，所以在你修改前需要清楚自己在做什么

## 系统类清单

下面是核心系统类的列表，他们在每次 CodeIgniter  启动时被调用：

-  Benchmark
-  Config
-  Controller
-  Exceptions
-  Hooks
-  Input
-  Language
-  Loader
-  Log
-  Output
-  Router
-  Security
-  URI
-  Utf8

## 替换核心系统类

要使用你自己的系统类替换默认类，只需将你的文件放到 *application/core/* 文件夹里：

	application/core/some_class.php

如果文件夹不存在，你可以创建一个。

只要你的自定义文件名和默认的完全一样，它就会自动替换原有的类。

需要注意的时，你的自定义类必须以 CI 作为前缀，例如你的文件名是 Input.php，那么类名就必须是：

	class CI_Input {

	}

## 扩展核心系统类

如果你仅仅是需要给存在的库添加一些函数，那完全没有必要替换整个库，仅需要扩展原生库。扩展一个类就像在一个类中添加例外：

-  类声明必须扩展自父类
-  你的新类名和文件名前缀必须是 MY\_（这是可配置的，见下面的说明）

例如，想要扩展原生 Input 类，你必须创建一个文件 **application/core/MY_Input.php**，并且声明类：

	class MY_Input extends CI_Input {

	}

注意: 如果在你的类里需要使用构造函数，必须在构造函数中显示的继承父类：

		class MY_Input extends CI_Input {

			public function __construct()
			{
				parent::__construct();
			}
		}

**提示:** 在你的新类中定义的所有函数，如果与父类中函数的命名完全一样,这些函数就能取代父类中原有的函数 (这也被称为"方法覆盖")。这允许你在本质上改变 CodeIgniter 的核心。

如果你扩展了控制器核心类，那么也要在你的应用程序控制器的构造函数里使用这个新类：

	class Welcome extends MY_Controller {

		public function __construct()
		{
			parent::__construct();
		}

		public function index()
		{
			$this->load->view('welcome_message');
		}
	}

## 自定义前缀

要设定你自己的子类前缀，请打开 *application/config/config.php* 文件，并找到：

	$config['subclass_prefix'] = 'MY_';

请注意：CodeIgniter 所有的库文件前缀都是 CI\_，所以你不要用。