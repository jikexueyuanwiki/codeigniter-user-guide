# 使用 CodeIgniter 库

所有可用的库文件都位于 *system/libraries/* 文件夹里。多数情况下，你需要在控制器中初始化后才能使用它们，方法如下：

	$this->load->library('class_name');

'class_name' 是你想要用的类名。例如加载“表单验证类”，可以这样做

	$this->load->library('form_validation');

一旦类库初始化后，你就可以按照用户手册中的方法来使用它们。

此外，给加载函数传递需要加载的多个类库数组，就可以同时加载多个类库。

例如:

	$this->load->library(array('email', 'table'));

## 创建你自己的类库

请阅读用户手册中关于[创建自己类库]文档。


[创建自己类库]: creating_libraries.md
