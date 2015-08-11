# 管理你的应用程序

默认情况下，你仅会用 CodeIgniter 管理一个应用程序，这个程序位于 *application/*
文件夹中。当然，也有可能多个程序共享一个 CodeIgniter，甚至对 *application/* 重命名或更换路径。


## 重命名应用文件夹

如果你想重命名应用文件夹，你可以打开 index.php 文件，并使用变量 `$application_folder` 设置它的名字。

	$application_folder = 'application';

## 更改应用文件夹路径

你可以将应用文件夹移动到服务器上不同的位置，而不是根目录。打开 index.php 并设置 `$application_folder` 变量为服务器的完整路径：

	$application_folder = '/path/to/your/application';

## 在一个 CodeIgniter 下运行多个应用

如果你想要多个应用程序共享一个 CodeIgniter， 可以将 application 下所有的文件夹放在不同的应用程序的文件夹内。

例如，你要建立两个应用程序 "foo" 和 "bar",你的应用程序文件夹的结构可能如下:

	applications/foo/
	applications/foo/config/
	applications/foo/controllers/
	applications/foo/libraries/
	applications/foo/models/
	applications/foo/views/
	applications/bar/
	applications/bar/config/
	applications/bar/controllers/
	applications/bar/libraries/
	applications/bar/models/
	applications/bar/views/

要选择某个应用程序，需要打开 index.php 文件，并设置 `$application_folder` 变量。例如，选择使用 "foo" 程序，可以这么做：

	$application_folder = 'applications/foo';

注意: 每个应用程序都需要自己的 index.php 文件，它会调用相应的应用。你可以任意命名 index.php。