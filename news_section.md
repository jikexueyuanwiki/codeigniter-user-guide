# 读取新闻

删一个章节中，我们通过写一个包含静态页面的类了解了这个架构的基本概念。通过添加自定义路由规则我们也重新梳理了 URI。现在我们开始介绍动态内容，并开始使用数据库

## 创建数据模型

数据库操作并不在控制器中（controller），而是在数据模型里，这样可以很容易的被复用。一般我们在数据模型中查询，插入，更新数据库信息。它表示你的数据。

打开 *application/models/* 文件夹并创建一个新的文件 *News_model.php*，添加以下代码。请确认你已经按照文档[数据库配置]了数据库。

	<?php
	class News_model extends CI_Model {

		public function __construct()
		{
			$this->load->database();
		}
	}

这段代码和之前的控制器代码很相似。通过继承 `CI_Model` 并加载数据库，创建了一个数据模型。通过 `$this->db` 对象可以使得数据库类可用。

在查询数据库前，需要创建一个数据表。连接数据库并运行 SQL 命令（MySQL)，并在里面添加内容。

	CREATE TABLE news (
		id int(11) NOT NULL AUTO_INCREMENT,
		title varchar(128) NOT NULL,
		slug varchar(128) NOT NULL,
		text text NOT NULL,
		PRIMARY KEY (id),
		KEY slug (slug)
	);

现在数据库和数据模型已经创建好了，我们需要一个方法把我们的文章从数据库中提取出来。为了完成这个目标，CodeIgniter 中包含了的抽象层：[查询生成器]。它能让你的查询语句只写一次并让它们工作（[支持的所有数据库])。添加以下代码到你的数据模型中。

	public function get_news($slug = FALSE)
	{
		if ($slug === FALSE)
		{
			$query = $this->db->get('news');
			return $query->result_array();
		}

		$query = $this->db->get_where('news', array('slug' => $slug));
		return $query->row_array();
	}

通过这段代码，你可以执行 2 种不同的查询。你可以得到所有新的记录，或者通过 `slug <#>` 得到新的记录。你可能已经注意到 `$slug` 变量在运行前并没有被验证过，因为[查询生成器]已经把这事弄完了。



## 显示新闻

既然已经写好了查询，我们就需要把这个数据模型和视图相关联。其实这个事情在我们之前写的 `Pages` 控制器中可以实现，但是为了更清楚的展示给大家，我们创建一个新的 `News` 控制器，这个控制器位于 *application/controllers/News.php*。

	<?php
	class News extends CI_Controller {

		public function __construct()
		{
			parent::__construct();
			$this->load->model('news_model');
			$this->load->helper('url_helper');
		}

		public function index()
		{
			$data['news'] = $this->news_model->get_news();
		}

		public function view($slug = NULL)
		{
			$data['news_item'] = $this->news_model->get_news($slug);
		}
	}

这段代码和我们之前写过的类似。首先，`__construct()` 方法调用了父类(`CI_Controller`) 的构造函数并加载了数据模型。这样它就能在这个控制器的其他方法中使用。同时也加载了 `URL Helper` 函数集合，它会在之后的视图中用到。

接着，有两个方法可以显示所有新闻和某个新闻。在第二个方法的参数是 `$slug` 。这个方法用 `$slug` 来确定返回哪个新闻数据。

现在，使用我们的数据模型检索除了数据，但是还没有显示任何内容。下一步就是将数据传给视图。

	public function index()
	{
		$data['news'] = $this->news_model->get_news();
		$data['title'] = 'News archive';

		$this->load->view('templates/header', $data);
		$this->load->view('news/index', $data);
		$this->load->view('templates/footer');
	}

上面的代码从数据模型中获取到所有的新闻，并将它赋值给变了。文章标题为 `$data['title']` ，所有的数据都会传递给视图。现在你需要创建一个视图来显示这些内容。创建 *application/views/news/index.php* 并添加以下代码。

	<h2><?php echo $title; ?></h2>
	
	<?php foreach ($news as $news_item): ?>

		<h3><?php echo $news_item['title']; ?></h3>
		<div class="main">
			<?php echo $news_item['text']; ?>
		</div>
		<p><a href="<?php echo site_url('news/'.$news_item['slug']); ?>">View article</a></p>

	<?php endforeach; ?>

这里，遍历所有的新闻并显示给用户。从上面的代码可以看出我们的模板是 PHP 和 HTML 的混合体。如果你想用模板语言，可以使用 CodeIgniter [模板解析器] 或者第三方解析器。

新闻列表页面已经完成，但是还没有新闻的独立显示页面。早些时候创建的数据模型可以很容易的实现这个功能。仅需要你添加以下代码到控制器并创建一个视图。回到 `News` 控制器并使用以下代码更新 `view()`。

	public function view($slug = NULL)
	{
		$data['news_item'] = $this->news_model->get_news($slug);

		if (empty($data['news_item']))
		{
			show_404();
		}

		$data['title'] = $data['news_item']['title'];

		$this->load->view('templates/header', $data);
		$this->load->view('news/view', $data);
		$this->load->view('templates/footer');
	}

现在这里将 `$slug` 变量作为参赛传给了 `get_news()` 方法，这样就可以方法想要的文章了。剩下需要做的事情仅仅是创建一个相应的视图 *application/views/news/view.php*。添加以下代码：

	<?php
	echo '<h2>'.$news_item['title'].'</h2>';
	echo $news_item['text'];

## 路由

因为之前设置了通配符路由规则，现在你需要额外的路由来显示刚刚写的控制器。按照以下代码修改你的路由文件 *application/config/routes.php*。它让你的请求到 `News` 控制器，而不是直达 `Pages` 控制器。第一行代码表示的是 `news` 控制器中通过 `slug` 读取的那条新闻。

	$route['news/(:any)'] = 'news/view/$1';
	$route['news'] = 'news';
	$route['(:any)'] = 'pages/view/$1';
	$route['default_controller'] = 'pages/view';

把浏览器地址定位到你的根目录，后面加上 index.php/news，然后看看你的新闻页面。


[数据库配置]: configuration.md
[查询生成器]: query_builder.md
[支持的所有数据库]: requirements.md
[模板解析器]:parser.md