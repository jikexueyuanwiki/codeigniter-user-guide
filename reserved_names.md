# 保留字

为了便于编码，CodeIgniter 使用了一系列的函数，方法，类和变量名来完成操作。因此，有一些名字开发中不能使用。底下的列表中的保留字不能被使用。

## 控制器名

因为你的控制器类继承自主程序控制器，你方法名一定不要和主程序控制器类中得函数名相同，否则你的局部方法将会覆盖他们。底下列出了保留的名字。不能在你的控制器中使用：


-  CI_Controller
-  Default
-  index

## 函数

-  :php:func:`is_php()`
-  :php:func:`is_really_writable()`
-  `load_class()`
-  `is_loaded()`
-  `get_config()`
-  :php:func:`config_item()`
-  :php:func:`show_error()`
-  :php:func:`show_404()`
-  :php:func:`log_message()`
-  :php:func:`set_status_header()`
-  :php:func:`get_mimes()`
-  :php:func:`html_escape()`
-  :php:func:`remove_invisible_characters()`
-  :php:func:`is_https()`
-  :php:func:`function_usable()`
-  :php:func:`get_instance()`
-  `_error_handler()`
-  `_exception_handler()`
-  `_stringify_attributes()`

## 变量

-  `$config`
-  `$db`
-  `$lang`

## 常量

-  ENVIRONMENT
-  FCPATH
-  SELF
-  BASEPATH
-  APPPATH
-  VIEWPATH
-  CI_VERSION
-  MB_ENABLED
-  ICONV_ENABLED
-  UTF8_ENABLED
-  FILE_READ_MODE
-  FILE_WRITE_MODE
-  DIR_READ_MODE
-  DIR_WRITE_MODE
-  FOPEN_READ
-  FOPEN_READ_WRITE
-  FOPEN_WRITE_CREATE_DESTRUCTIVE
-  FOPEN_READ_WRITE_CREATE_DESTRUCTIVE
-  FOPEN_WRITE_CREATE
-  FOPEN_READ_WRITE_CREATE
-  FOPEN_WRITE_CREATE_STRICT
-  FOPEN_READ_WRITE_CREATE_STRICT
-  SHOW_DEBUG_BACKTRACE
-  EXIT_SUCCESS
-  EXIT_ERROR
-  EXIT_CONFIG
-  EXIT_UNKNOWN_FILE
-  EXIT_UNKNOWN_CLASS
-  EXIT_UNKNOWN_METHOD
-  EXIT_USER_INPUT
-  EXIT_DATABASE
-  EXIT__AUTO_MIN
-  EXIT__AUTO_MAX
