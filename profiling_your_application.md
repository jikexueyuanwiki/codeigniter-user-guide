# 应用性能分析

这个分析器类将会显示基准结果，运行的查询，并将 `$_POST` 数据放在你的页尾。这个信息在开发中非常有用，它能帮你调试和优化。

## 初始化类

注意: 这个类不需要初始化。如果已按照下面的方式激活,他将被输出类自动装载。

## 启动分析器

在控制器中设置以下方法,可以启动该分析器。

	$this->output->enable_profiler(TRUE);

分析器启动后将产生一个报告并插入您的页面底部.

使用以下方法可以禁用分析器：

	$this->output->enable_profiler(FALSE);

## 设置基准点

为了让分析器编译并显示你的基准数据，你必须特定的语法命名基准点。

更多细节可以参考文档“基准库”

## 启用和禁用分析器的段

可以通过设置相应的控制变量 TRUE 或 FALSE来启用或禁用分析数据中得每个字段。它可以通过下面两种方法之一来实现。其中一个方法是你可以在 *application/config/profiler.php* 配置文件里设置整个程序的全局默认值。例如：

	$config['config']          = FALSE;
	$config['queries']         = FALSE;

在你的控制器中，你可以通过调用 `set_profiler_sections()` 方法来重写默认值和配置文件值。

	$sections = array(
		'config'  => TRUE,
		'queries' => TRUE
	);

	$this->output->set_profiler_sections($sections);

下表列出了可用的分析器数据字段和用来访问这些字段的key。

| 默认键值  | 描述  | 默认值 |
|:------------- |:---------------:| -------------:|
| benchmarks      | 在各个计时点花费的时间以及总时间 |         TRUE |
| config      | CodeIgniter 配置变量        |           TRUE |
| controller_info | 被调用的method及其所属的控制器类        |            TRUE |
| get | 在request中传递的所有 GET 参数        |            TRUE |
| http_headers | 本次请求的 HTTP 头        |            TRUE |
| memory_usage | 本次请求消耗的内存（byte 为单位）        |            TRUE |
| post | 在request中传递的所有POST参数        |            TRUE |
| queries | 列出执行的数据库操作语句及其消耗的时间       |            TRUE |
| uri_string | 本次请求的URI        |            TRUE |
| session_data | 数据存储在当前 session        |            TRUE |
| query_toggle_count | 指定显示多少个数据库查询语句，剩下的则默认折叠起来。        |            25 |


注意: 在你的数据库配置中禁用`保存查询`文档设置，将会有效的禁用数据库查询性能分析。你可以使用 `$this->db->save_queries = TRUE;` 重写这个设置。没有这个设置，你无法查看查询或者 `last_query`。