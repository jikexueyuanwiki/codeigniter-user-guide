# 创建辅助类

在某些情况下，你可能需要开发一个类，这个类能使用控制器一部分功能。

## get_instance()

* 返回:	你的控制器实例的引用
* 返回类型:	CI_Controller

**在你的控制器方法中初始化的任何类，都可以访问 CodeIgniter 原生资源**，简单通过使用 `get_instance()` 函数。这个函数回传 CodeIgniter 对象。

通常来说，调用任何 CodeIgniter 方法需要你使用 `$this` 结构：

	$this->load->helper('url');
	$this->load->library('session');
	$this->config->item('base_url');
	// etc.

然而，`$this` 仅能再你的控制器，数据模型，视图中使用。如果你想在你的类中使用 CodeIgniter 类，可以参考以下做法：

首先，将 CodeIgniter 对象赋值给变量：

	$CI =& get_instance();

一旦你将对象赋值给变量，你可以使用变量实例取代 `$this`：

	$CI =& get_instance();

	$CI->load->helper('url');
	$CI->load->library('session');
	$CI->config->item('base_url');
	// etc.

如果在其他类中使用 `get_instance()`，可以将它赋给一个属性。通过这个方法，你不需要在每个方法中调用 `get_instance()`。例如：

	class Example {

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
			$this->CI->config->item('base_url');
		}
	}

在上述的例子中，`foo()` 和 `bar()` 方法将会在你实例化的子类工作，不需要每次都调用 `get_instance()`。
