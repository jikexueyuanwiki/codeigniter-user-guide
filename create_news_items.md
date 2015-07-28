# 创建新闻

现在你已经知道如何使用 CodeIgniter 从数据库里读取数据，但你还没往数据库写任何东西。这个章节将会扩展你的之前创建的新闻控制器和模式包含这个功能。

## 创建一个表单

为了给数据库输入数据，你需要创建一个表单来存储信息。你的表单需要两个字段，标题和正文。你可以从数据模型中得标题来获得 slug。在 `application/views/news/create.php` 创建一个新的视图。

    <h2><?php echo $title; ?></h2>

    <?php echo validation_errors(); ?>

    <?php echo form_open('news/create'); ?>

        <label for="title">Title</label> 
        <input type="input" name="title" /><br />

        <label for="text">Text</label>
        <textarea name="text"></textarea><br />

        <input type="submit" name="submit" value="Create news item" /> 

    </form>

这里可能有 2 个事情你不太了解：form_open() 函数和 validation_errors() 函数。

第一个函数由[表单辅助函数]提供,用来提供表单元素和额外的附加函数，比如添加隐藏的[安全类]。另外一个用来报告验证表单正确性过程中的错误。

回到你的新闻控制器。在这里你需要做 2 件事情，一是检查表单是否已经提交，二是检查提交的数据能通过验证，你需要用到[表单验证]来坐这些事。

    public function create()
    {
        $this->load->helper('form');
        $this->load->library('form_validation');
        
        $data['title'] = 'Create a news item';
        
        $this->form_validation->set_rules('title', 'Title', 'required');
        $this->form_validation->set_rules('text', 'text', 'required');
        
        if ($this->form_validation->run() === FALSE)
        {
            $this->load->view('templates/header', $data);   
            $this->load->view('news/create');
            $this->load->view('templates/footer');
            
        }
        else
        {
            $this->news_model->set_news();
            $this->load->view('news/success');
        }
    }

上面的代码添加了一些功能，前几行代码载入了表单辅助函数和表单验证库。这样就设置好了表单验证规则。`set_rules()` 方法有3个参数：输入域的名称，错误信息名称，规则。这里的规则是标题和正文不能为空。

如上所示，CodeIgniter 拥有强大的表单验证库。详情可以阅读[表单验证]文档。

继续看，你可以发现一个用来检查表单验证是否运行成功的条件。如果失败，显示表单。如果成功提交并通过所有验证，视图将会显示消息。创建一个视图 application/views/news/success.php，并写入成功消息。

## 数据模型

最后需要做的事情就是将数据写到数据库中。你将会用到 Query Builder 类来插入一条信息，并使用输入库来获得 post 数据。打开之前创建的数据模型，并添加以下代码：

    public function set_news()
    {
        $this->load->helper('url');
        
        $slug = url_title($this->input->post('title'), 'dash', TRUE);
        
        $data = array(
            'title' => $this->input->post('title'),
            'slug' => $slug,
            'text' => $this->input->post('text')
        );
        
        return $this->db->insert('news', $data);
    }

这新方法用来维护插入到数据库的信息。第三行包含了一个新的函数 `url_title()`。这个函数由 [URL 辅助函数]提供，用来组织你输入的字符串，将空格的内容换成横线（-），确保其中全都是小写字母。这样就可以拥有一个漂亮的 slug，可以完美的创建 URI。

让我们继续准备将要向$data数组输入的记录。每个元素都和数据库表里的列相对应。你可能已经注意到了一个新方法 [post()]。这个方法可以保证数据是过滤过的，从而保护你不受其他人的恶意攻击。这个输入类默认加载。最后将 $data 数据插入到数据库中。

## 路由

在你添加新闻到 CodeIgniter 应用前，你必须给 `config/routes.php` 添加一个附件的规则。它需要保证你的文件包含以下代码。它能让 CodeIgniter 将 “create” 看做一个方法，而不是一个新闻的 slug。

    $route['news/create'] = 'news/create';
    $route['news/(:any)'] = 'news/view/$1';
    $route['news'] = 'news';
    $route['(:any)'] = 'pages/view/$1';
    $route['default_controller'] = 'pages/view';

现在可以将你的浏览器定位到安装了 CodeIgniter 本地开发环境，并添加 `index.php/news/create` 到URL。恭喜你创建了第一个 CodeIgniter 程序！添加一些新闻，然后看看以创建的不同页面吧。


[表单辅助函数]: form_helper.md
[安全类]:security.md
[表单验证]: form_validation.md
[URL 辅助函数]:url_helper.md
[post()]: input.md