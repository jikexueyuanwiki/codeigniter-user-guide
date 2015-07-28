# 使用 CodeIgniter 适配器

适配器是一种特殊的库，它拥有一个父类和任意数量的子类。子类可以访问父类，但是不能访问它的兄弟类。在控制器中，适配器为类库提供了一种优雅的语法，类库就会因此获益，或需要分解成离散的类。

适配器在 *system/libraries/* 文件夹里，命名一个和类名字相同的文件夹，文件夹下存放该类。同时在该文件夹中，有一个字文件夹叫做 drivers,它包含了所有可能存在的子类。

要使用一个适配器，你需要在一个控制器离用如下的初始化函数初始化它：

	$this->load->driver('class_name');

这里的 `class_name` 是你需要想要加载的适配器名字，比如说你想加载一个叫做 "Some_parent" 适配器，可以这么做：

	$this->load->driver('some_parent');

这个类的方法可以这么调用：

	$this->some_parent->some_method();

作为子类的适配器能直接通过父类调用，而不能初始化。

	$this->some_parent->child_one->some_method();
	$this->some_parent->child_two->another_method();

## 创建自己的适配器

如果想创建自己的适配器，请阅读[创建适配器]文档。

[创建适配器]: creating_drivers.md
