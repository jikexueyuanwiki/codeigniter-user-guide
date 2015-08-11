# PHP 开发规范

本章主要描述了开发 CodeIgniter 框架本身所采用的编码风格。建议在你的代码中也使用这些规范。

## 文件格式

文件需要保存为 Unicode (UTF-8)。不应该使用 BOM，与 UTF-16 和 UTF-32 不同，UTF-8 编码的文件不需要指明字节序，而且 BOM 在 PHP 中会产生预期之外的输出，阻止了应用程序设置它自己的头信息。应该使用 Unix 格式的行结束符(LF)。

以下是在一些常见的文本编辑器中更改这些设置的方法。针对你的编辑器，方法也许会有所不同；请参考你的编辑器的说明

### TextMate

* 打开 Application Preferences
* 点击 Advanced, 点击 "Saving" 栏
* 在 "File Encoding", 选择 "UTF-8 (推荐)"
* 在 "Line Endings", 选择 "LF (推荐)"
* *可选* 检查 "Use for existing files as well" ，如果你想修改文件结束符，打开你的新参考。

### BBEdit

* 打开 the Application Preferences
* 选择左边 "Text Encodings"
* 在 "Default text encoding for new documents", 选择 "Unicode (UTF-8,
   no BOM)"
* *可选:* 在 "If file's encoding can't be guessed, use", 选择
   "Unicode (UTF-8, no BOM)"
* 选择左侧 "Text Files".
* 在 "Default line breaks", 选择 "Mac OS X and Unix (LF)"

## PHP 闭合标签

PHP 闭合标签 “?>” 在 PHP 中对 PHP 的分析器是可选的。但是，如果使用闭合标签，任何由开发者，用户，或者 FTP 应用程序插入闭合标签后面的空格都有可能会引起多余的输出、php 错误、之后的输出无法显示、空白页。因此，所有的 php 文件应该省略这个 php 闭合标签，并插入一段注释来标明这是文件的底部并定位这个文件在这个应用的相对路径。这样有利于你确定这个文件已经结束而不是被删节的。

## 命名文件

类文件命名采用首字母大写，而其他文件命名（配置，视图，脚本等）必须全是小写

**错误的**：

	somelibrary.php
	someLibrary.php
	SOMELIBRARY.php
	Some_Library.php

	Application_config.php
	Application_Config.php
	applicationConfig.php

**正确的**:

	Somelibrary.php
	Some_library.php

	applicationconfig.php
	application_config.php

另外，类文件名必须和类名相同。例如，如果你又一个类 `Myclass`，它的文件名也必须是 **Myclass.php**

## 类和方法命名

类名必须大写开头，字与字间使用下划线分割，而不是驼峰风格。

**错误**:

	class superclass
	class SuperClass

**正确**:

	class Super_class
	class Super_class {

		public function __construct()
		{

		}
	}

类方法必须全小写，命名能清楚的表达函数的功能，最好包含动词。尽量避免太长太啰嗦的命名。字与字间使用下划线分割。

**错误**:

	function fileproperties()		// not descriptive and needs underscore separator
	function fileProperties()		// not descriptive and uses CamelCase
	function getfileproperties()		// Better!  But still missing underscore separator
	function getFileProperties()		// uses CamelCase
	function get_the_file_properties_from_the_file()	// wordy

**正确**:

	function get_file_properties()	// descriptive, underscore separator, and all lowercase letters

## 变量名

变量的命名规则与方法的命名规则十分相似。就是说，变量名应该只包含小写字母，用下划线分隔，并且能适当地指明变量的用途和内容。那些短的、无意义的变量名应该只作为迭代器用在 for() 循环里。

**错误**:

	$j = 'foo';		// single letter variables should only be used in for() loops
	$Str			// contains uppercase letters
	$bufferedText		// uses CamelCasing, and could be shortened without losing semantic meaning
	$groupid		// multiple words, needs underscore separator
	$name_of_last_city_used	// too long

**正确**:

	for ($j = 0; $j < 10; $j++)
	$str
	$buffer
	$group_id
	$last_city

## 注释

通常来说，代码尽量需要注释。它不仅能帮助新手描述清楚流程和代码目的，也能让你几个月后再看代码能很快入手。注释非强制规范，但是推荐以下形式。

[文档块](http://manual.phpdoc.org/HTMLSmartyConverter/HandS/phpDocumentor/tutorial_phpDocumentor.howto.pkg.html#basics.docblock)式的注释要写在类和方法的声明前，这样它们就能被集成开发环境(IDE)捕获：

	/**
	 * Super Class
	 *
	 * @package	Package Name
	 * @subpackage	Subpackage
	 * @category	Category
	 * @author	Author Name
	 * @link	http://example.com
	 */
	class Super_class {



	/**
	 * Encodes string for use in XML
	 *
	 * @param	string	$str	Input string
	 * @return	string
	 */
	function xml_encode($str)



	/**
	 * Data for class manipulation
	 *
	 * @var	array
	 */
	public $data = array();

使用行注释时，在大的注释块和代码间留一个空行。

	// break up the string by newlines
	$parts = explode("\n", $str);

	// A longer comment that needs to give greater detail on what is
	// occurring and why can use multiple single-line comments.  Try to
	// keep the width reasonable, around 70 characters is the easiest to
	// read.  Don't hesitate to link to permanent external resources
	// that may provide greater detail:
	//
	// http://example.com/information_about_something/in_particular/

	$parts = $this->foo($parts);

## 常量

常量命名除了要全部用大写外，其他的规则都和变量相同。**在适当的时候，始终使用 CodeIgniter 常量，例如 LASH, LD, RD, PATH_CACHE 等等.**

**错误**:

	myConstant	// missing underscore separator and not fully uppercase
	N		// no single-letter constants
	S_C_VER		// not descriptive
	$str = str_replace('{foo}', 'bar', $str);	// should use LD and RD constants

**正确**:

	MY_CONSTANT
	NEWLINE
	SUPER_CLASS_VERSION
	$str = str_replace(LD.'foo'.RD, 'bar', $str);

## TRUE, FALSE, 和 NULL

**TRUE**, **FALSE**, 和 **NULL** 关键字必须全部大写。

**错误**:

	if ($foo == true)
	$bar = false;
	function foo($bar = null)

**正确**:

	if ($foo == TRUE)
	$bar = FALSE;
	function foo($bar = NULL)

## 逻辑操作

不建议使用 `||` ，因为在某些设备上辨识度低（很像数字 11）。`&&` 比 `AND` 更好一些，`!` 前后都要加上空格。

**错误**:

	if ($foo || $bar)
	if ($foo AND $bar)  // okay but not recommended for common syntax highlighting applications
	if (!$foo)
	if (! is_array($foo))

**正确**:

	if ($foo OR $bar)
	if ($foo && $bar) // recommended
	if ( ! $foo)
	if ( ! is_array($foo))
	

## 比较返回值与类型映射

某些 PHP 函数失败时返回 FALSE，但是也有可能返回 "" 或 0，它在松散的比较中会被计算为 FALSE。在条件语句中使用这些返回值的时候，为了确保返回值是你所预期的类型而不是一个有着松散类型的值，请进行显式的比较。

在返回和检查你自己的变量时也要遵循这种严格的方法，必要时使用 === 和 !==。

**错误**:

	// If 'foo' is at the beginning of the string, strpos will return a 0,
	// resulting in this conditional evaluating as TRUE
	if (strpos($str, 'foo') == FALSE)

**正确**:

	if (strpos($str, 'foo') === FALSE)

**错误**:

	function build_string($str = "")
	{
		if ($str == "")	// uh-oh!  What if FALSE or the integer 0 is passed as an argument?
		{

		}
	}

**正确**:

	function build_string($str = "")
	{
		if ($str === "")
		{

		}
	}


## 调试代码

提交代码的时候不要遗留调试信息，即使是注释过的。`var_dump()`, `print_r()`, `die()`/`exit()` 不要出现在你的代码中，除非有特殊用途.

## 文件中的空格

在 PHP 开始标签签名和结束标签后面都不应该有空格。由于输出已经被缓存，所以文件中的空格会导致 CodeIgniter 在输出自己内容前，就开始输出，这会导致 CodeIgniter 出错，并且无法发送正确的头。

## 兼容性

CodeIgniter 推荐的版本是 5.4+，但是也需要和 PHP 5.2.4 版本兼容。你的代码必须版本兼容，或提供合适的备案，或者做出选择性的功能。

不要使用那些依赖于非默认安装库的 PHP 函数，除非你的代码中包含了该函数不可用时的替代方法。


## 一个文件一个类

每个类都有独立的文件，除非这些类紧密关联。CodeIgniter 里 Xmlrpc 库就是一个文件多个类。

## 空格

在你的使用 tab 作为缩进，而不是空格。这是一件很小的事情，但是它能让其他开发者使用他们喜欢的缩进排版开阅读你的代码，并且可以在他们自己的 IDE 中调整。另外还有一个好处，使用一个 tab 至少能取代4个空格，因此文件更小。	

## 换行

文件必须使用 Unix 的换行。这个规则比较偏向于 windows 使用者，总之确认你的编辑器是用的 unix 换行。

## 代码缩进

除了类声明，使用 Allman 风格缩进，大括号永远都是独自一行，并且与其所属的控制语句有相同的缩进排版。

**错误**:

	function foo($bar) {
		// ...
	}

	foreach ($arr as $key => $val) {
		// ...
	}

	if ($foo == $bar) {
		// ...
	} else {
		// ...
	}

	for ($i = 0; $i < 10; $i++)
		{
		for ($j = 0; $j < 10; $j++)
			{
			// ...
			}
		}
		
	try {
		// ...
	}
	catch() {
		// ...
	}

**正确**:

	function foo($bar)
	{
		// ...
	}

	foreach ($arr as $key => $val)
	{
		// ...
	}

	if ($foo == $bar)
	{
		// ...
	}
	else
	{
		// ...
	}

	for ($i = 0; $i < 10; $i++)
	{
		for ($j = 0; $j < 10; $j++)
		{
			// ...
		}
	}
	
	try 
	{
		// ...
	}
	catch()
	{
		// ...
	}

## 括号间距

一般来说，括号不应该有额外的空格，但是在一些需要括号来接受参数的控制结构（declare, do-while,elseif, for, foreach, if, switch, while）后面应该加上空格，以便于函数区别，增加可读性。

**错误**:

	$arr[ $foo ] = 'foo';

**正确**:

	$arr[$foo] = 'foo'; // no spaces around array keys

**错误**:

	function foo ( $bar )
	{

	}

**正确**:

	function foo($bar) // no spaces around parenthesis in function declarations
	{

	}

**错误**:

	foreach( $query->result() as $row )

**正确**:

	foreach ($query->result() as $row) // single space following PHP control structures, but not in interior parenthesis

## 本地化文本

CodeIgniter 库需要使用相应的语言文件。

**错误**:

	return "Invalid Selection";

**正确**:

	return $this->lang->line('invalid_selection');

## 私有的方法和变量

仅能内部访问的方法和变量，比如工具和辅助函数，必须用下划线开头。

	public function convert_text()
	private function _convert_text()

## PHP 错误

代码不应该有错误，并且不能通过隐藏警告和提醒来达成目标。用到不是自己创建的变量时（比如，`$_POST` 数组 key），先用 `isset()` 检查是否可用。

确保你的开发环境允许所有的用户报告错误，以及启用 PHP 环境中得 display_errors。你可以这么检查：

	if (ini_get('display_errors') == 1)
	{
		exit "Enabled";
	}

在某些服务器 *display_errors* 被禁用，而你没有办法修改 php.ini，你通常可以这么启用：

	ini_set('display_errors', 1);

注意: 运行的时候使用 `ini_set()` 来设置 [display_errors](http://php.net/manual/en/errorfunc.configuration.php#ini.display-errors).也就是说程序发生重大错误的时候，将不会有任何作用。

## 短标记

一直使用 PHP 完整标记，以避免服务器不支持短标记，也就是未打开 short_open_tag 。

**错误**:

	<? echo $foo; ?>

	<?=$foo?>

**正确**:

	<?php echo $foo; ?>

注意: PHP 5.4 起，永远可以使用 **<?=** 标签。

## 每行一条语句

永远不要在一行里写多条语句

**错误**:

	$foo = 'this'; $bar = 'that'; $bat = str_replace($foo, $bar, $bag);

**正确**:

	$foo = 'this';
	$bar = 'that';
	$bat = str_replace($foo, $bar, $bag);

## 字符串

一直使用单引号，除非你需要解析变量，如果需要解析变量请使用大括号，以避免变量解析错误。 如果字符串包含单引号的话你可以使用双引号，这样就不用转义了。

**错误**:

	"My String"					// no variable parsing, so no use for double quotes
	"My string $foo"				// needs braces
	'SELECT foo FROM bar WHERE baz = \'bag\''	// ugly

**正确**:

	'My String'
	"My string {$foo}"
	"SELECT foo FROM bar WHERE baz = 'bag'"

## SQL 查询

SQL 关键字永远大写: SELECT, INSERT, UPDATE, WHERE, AS, JOIN, ON, IN, etc.

将较长的语句拆成多行可以增加可读性，最好每个子句都放一行

**错误**:

	// keywords are lowercase and query is too long for
	// a single line (... indicates continuation of line)
	$query = $this->db->query("select foo, bar, baz, foofoo, foobar as raboof, foobaz from exp_pre_email_addresses
	...where foo != 'oof' and baz != 'zab' order by foobaz limit 5, 100");

**正确**:

	$query = $this->db->query("SELECT foo, bar, baz, foofoo, foobar AS raboof, foobaz
					FROM exp_pre_email_addresses
					WHERE foo != 'oof'
					AND baz != 'zab'
					ORDER BY foobaz
					LIMIT 5, 100");

## 默认的函数参数

适当的时候，提供函数参数的缺省值，这有助于防止因错误的函数调用引起的 PHP 错误，另外提供常见的备选值可以节省几行代码 例如:

	function foo($bar = '', $baz = FALSE)
