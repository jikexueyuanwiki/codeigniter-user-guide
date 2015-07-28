# 疑难解答

如果发现无论你你在 URL 里面写什么，都只是出现默认页面的话，有可能是你的服务器不支持 PATH_INFO 变量，它用来给搜索引擎提供友好的 URL。解决这个问题的第一步是打开 `application/config/config.php` 文件，查找 URI Protocol 信息,你可以试试其他的设置方法。如果这些方法都无效，你需要让 CodeIgniter 去强行加一个问号去标记你的 URL。为了做到这点，打开你的 **application/config/config.php** 文件，把:

	$config['index_page'] = "index.php";

改成这样：

	$config['index_page'] = "index.php?";
