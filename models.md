# 模型

模型对于那些想使用传统的 MVC 方法的人来说是可选的。

## 什么是模型

模型是一个 PHP 类，是用来和数据库打交道的。例如，我们假设你使用 CodeIgniter 来管理你的博客。你应该会有一个模型类来插入，更新，检索博客数据。下面的例子将向你展示一个普通的模型类。

	class Blog_model extends CI_Model {

		public $title;
		public $content;
		public $date;

		public function __construct()
		{
			// Call the CI_Model constructor
			parent::__construct();
		}

		public function get_last_ten_entries()
		{
			$query = $this->db->get('entries', 10);
			return $query->result();
		}

		public function insert_entry()
		{
			$this->title	= $_POST['title']; // please read the below note
			$this->content	= $_POST['content'];
			$this->date	= time();

			$this->db->insert('entries', $this);
		}

		public function update_entry()
		{
			$this->title	= $_POST['title'];
			$this->content	= $_POST['content'];
			$this->date	= time();

			$this->db->update('entries', $this, array('id' => $_POST['id']));
		}

	}

注意: 上述例子使用的是`查询生成器`数据库方法。

注意: 为了方便，我们直接使用了 `$_POST` 方法。这不是一个好方法，平时我们应该使用输入库方法 `$this->input->post('title')`。

## 剖析模型

模型类文件存放在 *application/models/* 文件夹里。如果你想要这种类型的组织，可以在里面创建子文件夹。

最基本的模型类如下：

	class Model_name extends CI_Model {

		public function __construct()
		{
			parent::__construct();
		}

	}

**Model_name** 是你的类名。类名首字母必须大写其他字母小写。确保你的类继承了基本模型类。

文件名必须和类名匹配。例如，假如这是你的类：

	class User_model extends CI_Model {

		public function __construct()
		{
			parent::__construct();
		}

	}

类的文件名应该是：

	application/models/User_model.php

## 加载一个模型

模型可以在控制类中加载并调用。可以使用以下方法来加载模型类：

	$this->load->model('model_name');

如果你的模型在子文件夹中，需要包含模型文件夹的相对路径。例如，如果你在 *application/models/blog/Queries.php* 拥有一个模型，可以这么加载：

	$this->load->model('blog/queries');

一旦加载，你可以使用一个和你的类同名的对象来访问模型方法：

	$this->load->model('model_name');

	$this->model_name->method();

如果你想让你的模型有不同的对象名，可以通过指定加载方法第二个参数：

	$this->load->model('model_name', 'foobar');

	$this->foobar->method();

这里有个控制器的例子，加载一个模型，然后通过视图显示出来：

	class Blog_controller extends CI_Controller {

		public function blog()
		{
			$this->load->model('blog');

			$data['query'] = $this->blog->get_last_ten_entries();

			$this->load->view('blog', $data);
		}
	}
	

## 自动加载模型

如果你觉得需要一个特殊的模型，贯穿整个应用，可以告诉 CodeIgniter 初始化的时候自动加载。实现的方法是打开 *application/config/autoload.php* 文件，然后添加到自动加载数组。

## 连接到数据库

如果一个模型已经加载，但是没有自动连接到数据库。下面的连接选项可用：

-  您可以使用标准方法来连接数据库, 也可以通过控制器或者您的模型类。
-  您可以把第三个参数设置为 TRUE，来使模型加载函数自动连接数据库，连接配置可以在您的数据库配置文件中可以定义


	`$this->load->model('model_name', '', TRUE);`


-  您可以手动设定第三个参数，来加载您的自定义数据库配置:
	
		$config['hostname'] = 'localhost';
		$config['username'] = 'myusername';
		$config['password'] = 'mypassword';
		$config['database'] = 'mydatabase';
		$config['dbdriver'] = 'mysqli';
		$config['dbprefix'] = '';
		$config['pconnect'] = FALSE;
		$config['db_debug'] = TRUE;
	
		$this->load->model('model_name', '', $config);
