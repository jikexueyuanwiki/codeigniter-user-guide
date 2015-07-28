# 钩子 - 扩展系统核心

CodeIgniter 的钩子功能，让你可以在不改变系统核心文件的基础上，改变或增加系统的核心运行功能。当 CodeIgniter 运行后，它会产生一个特殊的进程，这个进程在项目流程文件里有说明。当然，您可以自定义一些动作来替代程序运行过程中的某些阶段。例如，你可以在控制器加载前运行一段脚本，或者在加载后运行，或者你想在其他地方触发脚本。

## 启用钩子

通过设定 **application/config/config.php** 文件里的选项，钩子功能可以在全局范围内打开或者关闭：

	$config['enable_hooks'] = TRUE;

## 定义钩子

钩子定义在 **application/config/hooks.php** 里。每个钩子可以用下面格式的数组来定义：

	$hook['pre_controller'] = array(
		'class'    => 'MyClass',
		'function' => 'Myfunction',
		'filename' => 'Myclass.php',
		'filepath' => 'hooks',
		'params'   => array('beer', 'wine', 'snacks')
	);

**说明:**

数组的索引与你要用的钩子名相关。上述的例子中，挂钩点是 `pre_controller`。挂钩点参数列表如下所示，以下各项将定义在你相关联的钩子数组里：

-  **class** 你希望调用的类。如果想使用过程函数来代替类的话，此项为空。
-  **function** 你想调用的函数或方法的名字
-  **filename** 包含你的类或函数的文件名
-  **filepath** 包含你的脚本的文件夹注意:你的脚本必须放在 *application/* 下的目录里，所以，文件路径是相对于目录的。例如，如果你的脚本放在 *application/hooks/*里，你可以简单的使用 'hooks' 作为文件路径。如果脚本位于 *application/hooks/utilities/*，你可以把 'hooks/utilities' 作为你的文件路径。注意后面没有 "/"。
-  **params** 你希望传递给脚本的任何参数。这个参数是可选的。

如果你用的时 PHP 5.3+, 你也能使用 lambda/anoymous 函数
(或闭包（closures）) 作为钩子:

	$hook['post_controller'] = function()
	{
		/* do something here */
	};

## 多次调用同一个钩子

如果你在多个脚本中用同一个钩子，最简单的方法就是把你的数组定位成二维的，比如：

	$hook['pre_controller'][] = array(
		'class'    => 'MyClass',
		'function' => 'MyMethod',
		'filename' => 'Myclass.php',
		'filepath' => 'hooks',
		'params'   => array('beer', 'wine', 'snacks')
	);

	$hook['pre_controller'][] = array(
		'class'    => 'MyOtherClass',
		'function' => 'MyOtherMethod',
		'filename' => 'Myotherclass.php',
		'filepath' => 'hooks',
		'params'   => array('red', 'yellow', 'blue')
	);

注意每个数组后的中括号：

	$hook['pre_controller'][]

这样你就可以在多个脚本中使用同一个钩子。你定义的数组顺序就是程序的执行顺序。

## 挂钩点

以下是可用的挂钩点：

-  **pre_system**
   系统执行的早期执行，仅仅在 benchmark 和 hooks 类加载完毕的时候可用。而且没有路由或其他进程发生。
-  **pre_controller**
   在调用控制器之前可以挂钩。所有的基础类，路由，和安全性检测完成后。
-  **post_controller_constructor**
   初始化控制器成功后，以及其他方法调用前挂钩。
-  **post_controller**
   在控制器完全执行后调用。
-  **display_override**
   覆盖 `_display()` 方法，在系统执行的最后发送页面给浏览器。允许你使用自己的显示方法。注意，你需要使用 `$this->CI =& get_instance()` 引用 CI 的 superobject，然后最终数据可以通过调用 `$this->CI->output->get_output()` 获得。
-  **cache_override**
   可以让你调用自己的函数来取代 output 类中的 `_display_cache()` 函数。这可以让你使用自己的缓存显示机制。
-  **post_system**
   在最终渲染页面发送到浏览器后，浏览器接收完最终数据的系统末尾调用。