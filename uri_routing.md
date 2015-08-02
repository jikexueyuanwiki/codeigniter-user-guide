# URI 路由

一般来说，URL 字符串和相应的控制器类/方法一一对应。URI里的内容通常都是这个模式：

	example.com/class/function/id/

然而在一些例子中，你也许想要重新隐射这个关系来调用一个不同的类/方法（class/function),而不是与 URL 一一对应。

例如, 比如你想要 URL 长成这样:

	example.com/product/1/
	example.com/product/2/
	example.com/product/3/
	example.com/product/4/

通常 URL 的第二段表示方法名，但是上面的列子第二段表示产品 ID。为了完成这个目标，CodeIgniter 允许你重新映射。

## 设置你自己的路由规则

路由规则定义在 *application/config/routes.php* 文件里。在这个文件里，你可以找到一个名为 `$route` 的数组，它可以让你定义自己的路由规则。定义可以用两种方法：通配符和正则表达式。

### 通配符

一个典型的通配符通常长成这样：

	$route['product/:num'] = 'catalog/product_lookup';

在一个路由中，数组中的键需要匹配的 URI，而数组值包含将被重定向的目的地。在上述的例子中，如果在 URL 第一段中有 "product" ，第二段中是数字，将会用 "catalog" 类和 "product_lookup" 方法替换。

你可以匹配文字的值或者使用以下两种通配类型：

**(:num)** 将匹配一个只包含数字的段

**(:any)** 将会匹配一个包含任何字符的段（除了 '/' 字符，因为它是段分割器）

注意: 通配符实际是常规表达式的别名，**:any** 被翻译成 **[^/]+**，**:num** 被翻译成 **[0-9]+**。

注意: 路由将会按照定义的顺序来运行.高层的路由总是优先于低层的路由.

注意: 路由规则不是过滤器！例如：设定规则 'foo/bar/(:num)' 不会阻止控制器 *Foo* 和方法 bar* 被直接调用。

### 例子

以下是一些路由的例子：

	$route['journals'] = 'blogs';

如果 URL 的第一个分段（类名）是关键字 “journals”，将会被定向到 "blogs" 类。

	$route['blog/joe'] = 'blogs/users/34';

如果 URL 前两个分段是 "blog" 和 "joe"，那么将会重定向到 "blogs" 类的 "users" 方法中处理。并将 ID "34" 设为参数.

	$route['product/(:any)'] = 'catalog/product_lookup';

如果 URL 第一段是 "product"，第二段是任意字符，它将会被映射到 "catalog" 类的 "product_lookup" 方法。

	$route['product/(:num)'] = 'catalog/product_lookup_by_id/$1';

如果 URL 第一段是 "product"，第二段是任意数字，它将会被映射到 "catalog" 类的 "product_lookup_by_id" 方法，参数为 $1。

注意: 不要在前面或后面加 "/"。

### 正则表达式

你可以用正则表达式来自定义你的路由规则，任何有效的正则表达式都是运行的，甚至逆向引用。

注意: 如果你使用逆向引用，需要用双反斜线语法替换卫美元符语法（\\1 替换为 $1）

一个典型的正则表达式通常长成这样：

	$route['products/([a-z]+)/(\d+)'] = '$1/id_$2';

上例中，类似 `products/shirts/123` 的 URI，将会换成调用 "shirts" 控制器类和 "id_123" 方法。

使用正则表达式，你也可以获取斜线('/')中的段，这个代表中间多段的分隔符。

例如，如果一个用户访问你的 web 应用的被保护区域的密码，你希望他们在登陆后重新定向到同一个页面，你可以参考以下例子：

	$route['login/(.+)'] = 'auth/login/$1';

如果你不了解正则表达式，并想学习，可以参考 [regular-expressions.info](http://www.regular-expressions.info/).

注意: 你也可以混合使用正则表达式混合通配符。

### 回调

如果你的 PHP 版本 >= 5.3，你可以使用回调函数来取代一般的路由规则来处理 back-references，例如：

	$route['products/([a-zA-Z]+)/edit/(\d+)'] = function ($product_type, $id)
	{
		return 'catalog/product_edit/' . strtolower($product_type) . '/' . $id;
	};

### 路由中使用 HTTP 动作

可以使用 HTTP 动作（请求方法）来重新定义你的路由规则。当你建立 RESTful 应用的时候特别有用。你可以使用标准的 HTTP 动作（GET, PUT, POST, DELETE, PATCH），或自定义动作（比如 PURGE）。HTTP 动作 规则大小写敏感。所有你需要做的事情就是添加动作数组 key 到你的路由中。例如:

	$route['products']['put'] = 'product/insert';

在上述的例子中，PUT 请求 URI "products" 将会调用 `Product::insert()` 控制器方法.

	$route['products/(:num)']['DELETE'] = 'product/delete/$1';

DELETE 请求到 URL，第一段是 "products"，第二段数字将会映射到 `Product::delete()` 方法，传入数字到第一个参数上。

使用 HTTP 动作是可选的。

### 保留的路由

有三个保留的路由：

	$route['default_controller'] = 'welcome';

这个路由指定在 URI 里没有任何数据时，加载哪个控制器，大家载入根 URL 时就是这个情况。在上述的例子中，"welcome" 类将会被载入。你要尽量有一个预设路由，否则预设会出现一个 404 页面。

	$route['404_override'] = '';

如果请求的控制器没有找到，这个路由将会指示加载哪个控制器。它将会重写默认的 404 错误页面。它不会影响 `show_404()` 函数，这个函数将会继续加载默认的 *application/views/errors/error_404.php* 里的 *error_404.php* 文件。

	$route['translate_uri_dashes'] = FALSE;

很显然这是 boolean 值，这不是真的路由。这个选项让你自动的在控制器和方法中 URI 片段将 '-' 替换成下划线，从而让你节省更多的路由项目。这是必须的，因为破折号不是一个有效的类或方法名，如果你使用破折号，将会导致重大错误。

注意: 保留的路由必须在任何通配符或正则表达式前定义。