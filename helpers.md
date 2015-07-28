# 辅助函数

辅助函数能帮助你完成任务。每个辅助函数文件都是某个分类的函数集。其中 **URL
Helpers** 帮助我们创建链接，**Form Helpers** 帮助我们创建元素，**Text Helpers** 提供一些列的格式化输出，**Cookie Helpers** 设置并读取 cookies，**File Helpers** 帮助我们处理文件，等等。

和 CodeIgniter 中的其他部分不同，辅助函数不是通过面向对象方法实现。它们仅仅是简单的过程处理函数，每个辅助函数处理一个特定的任务，并且和其他函数没有依赖性。

CodeIgniter 默认不载入辅助函数文件，所以使用前需要加载。加载后就全局可用。

辅助函数通常存储在 **system/helpers**，或者 **application/helpers** 文件夹里。CodeIgniter 首先会查找 **application/helpers** 文件夹。如果文件夹里不存在，或者文件夹里没有找到，CodeIgniter 将会到 **system/helpers** 里查找。

## 加载辅助函数

加载辅助函数文件非常简单，如下所示：

	$this->load->helper('name');

其中 **name** 是辅助函数的文件名，没有 .php 文件扩展名。

例如，加载 **URL Helper** 文件，命名为 **url_helper.php**，可以这么做：

	$this->load->helper('url');

辅助函数可以在你的控制器方法里的任何地方加载（也可以在你的视图文件里加载，不过这不是好方法），建议你在使用辅助函数前加载它。你可以在控制器的构造函数中加载辅助函数，这样你可以在控制器的任何函数中都可以使用，你也可以在某个需要它的函数里加载。

注意: 辅助函数并不返回任何值，所以不要将它赋值给任何变量，直接想例子中那样用就可以了。

## 加载多个辅助函数

如果你需要加载多个辅助函数，你可以将他们加入到数组中，如下：

	$this->load->helper(
		array('helper1', 'helper2', 'helper3')
	);

## 自动加载辅助函数

如果需要某个辅助函数在你的应用任何地方都可以使用，你可以告诉 CodeIgniter 在系统初始化的时候自动加载。打开 **application/config/autoload.php** 文件，并加入需要的辅助函数。


## 使用辅助函数

一旦加载了想要用到的辅助函数文件，你就可以用标准的函数方法来调用它。

例如，在你的某个视图文件中使用 `anchor()` 函数创建链接，可以这么做：

	<?php echo anchor('blog/comments', 'Click Here');?>

"Click Here" 是连接名称，"blog/comments" 是链接到控制器或方法的 URI。

## ”扩展”辅助函数

如果你想**扩展**辅助函数，在 **application/helpers/** 文件夹里创建一个文件，它的名字和需要扩展的辅助函数一致，但是前缀是 **MY\_**（这是可以配置的，如下）。

如果你仅仅是想给原有的辅助函数添加一些方法，或者改变一个方法，就不需要重写自己的辅助函数，仅需要**扩展**它。

注意: “扩展” 这个词其实不太恰当，因为 Helper 的方法是过程式和离散式的，在传统的语言环境中无法被**扩展**，不过在 CodeIgniter 中，你可以添加或者修改 helper 方法。

例如，扩展本地已有的 **Array Helper** 你应该建立一个文件：**application/helpers/MY_array_helper.php**，并添加或重写函数：

	// any_in_array() is not in the Array Helper, so it defines a new function
	function any_in_array($needle, $haystack)
	{
		$needle = is_array($needle) ? $needle : array($needle);

		foreach ($needle as $item)
		{
			if (in_array($item, $haystack))
			{
				return TRUE;
			}
	        }

		return FALSE;
	}

	// random_element() is included in Array Helper, so it overrides the native function
	function random_element($array)
	{
		shuffle($array);
		return array_pop($array);
	}

## 设置你自己的前缀

为**扩展**辅助函数的而加的文件名前缀，同时也是对库和核心类的扩展。打开文件 **application/config/config.php**，然后找到以下内容：

	$config['subclass_prefix'] = 'MY_';

请注意所有本地的 CodeIgniter 库的前缀都是 **CI\_**，所以你不要用这个。

## 现在可以做什么？

你可以看看在目录里所有的辅助函数它们都是做什么的。
