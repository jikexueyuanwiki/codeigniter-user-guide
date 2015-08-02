# 兼容性函数

CodeIgniter 提供了一套兼容性函数，让你可以使用非原生的 PHP 函数，但是只有在更高的版本或者依赖于某个扩展插件。

作为定制化实现，这些函数也有一定的依赖性，但是如果你的安装的 PHP 没有提供原生函数，它们还是很有用的。

注意: 大部分和 `通用函数` 很像, 只要依赖性满足，兼容性函数一直可用。  <div class="custom-index container"></div>

## 哈希密码

这套函数提供一个 PHP 标准“哈希密码扩展” 的 “反向移植” ，仅在 PHP 5.5 版本之后可用。

### 依赖性

- PHP 5.3.7
- ``CRYPT_BLOWFISH`` 支持 ``crypt()``

### 常量

- ``PASSWORD_BCRYPT``
- ``PASSWORD_DEFAULT``

### 函数参考

#### password\_get\_info($hash)

* 参数 字符串 $hash: 哈希密码
* 返回：哈希过的密码信息
* 返回类型：数组

更多信息，可以参考 `PHP password_get_info() 使用手册`

#### password_hash($password, $algo[, $options = array()])

* 参数 字符串	$password: 文本密码
* 参数 整数	$algo: Hashing 算法
* 参数 数组	$options: Hashing 参数
* 返回	哈希过的密码，失败时返回 FALSE
* 返回类型	字符串

更多信息，可用参考 `PHP password_get_info() 使用手册`。

注意: 除非你提供自己的（有效的）salt，这个函数可用进一步的提供依赖于可用的 CSPRNG 源。满足底下的每个条件：

		- ``mcrypt_create_iv()`` with ``MCRYPT_DEV_URANDOM``
		- ``openssl_random_pseudo_bytes()``
		- /dev/arandom
		- /dev/urandom

#### password\_needs\_rehash()

* 参数 字符串	$hash: 哈希密码
* 参数 整数	$algo: 哈希算法
* 参数 数组	$options: 哈希参数
* 返回 TRUE 如果哈希符合给定的算法和参数，返回 TRUE，否则返回 FALSE
* 返回类型	bool

更多信息，可以参考 `PHP password_needs_rehash() 使用手册`。

#### password_verify($password, $hash)

* 参数 字符串	$password: 纯文本密码
* 参数 字符串	$hash: 哈希密码
* 返回	TRUE 如果密码和哈希匹配返回 TRUE，否则返回 FALSE。
* 返回类型	bool

更多信息，可以参考 `PHP password_verify() 使用手册`

## 哈希 (消息摘要)

这个兼容性层包含了 `hash_equals()` 和 `hash_pbkdf2()` 函数的公钥，除此之外还分别要求 PHP 5.6 和/或 5.5。

### 依赖

- 无

### 函数参考

#### hash_equals($known_string, $user_string)

* 参数 字符串	$known_string: 已知字符串
* 参数 字符串	$user_string: 用户提供的字符串
* 返回	如果字符串匹配返回 TRUE，否则返回 FALSE
* 返回类型	字符串

更多信息，可以参考 `PHP hash_equals() 使用手册`。

#### hash\_pbkdf2($algo, $password, $salt, $iterations[, $length = 0[, $raw_output = FALSE]])

* 参数 字符串	$algo: 哈希算法
* 参数 字符串	$password: 密码
* 参数 字符串	$salt: 哈希 salt
* 参数 整数	$iterations: 迭代过程中执行的次数
* 参数 整数	$length: 输出字符串的长度
* 参数 bool	$raw_output: 是否返回原始二级制数据
* 返回	成功返回加密过的值，失败返回 FALSE
* 返回类型	字符串

更多信息，可以参考 `PHP hash_pbkdf2() 使用手册`。

## 多字节字符串

这套兼容性函数提供为多字节字符串扩展提供有限支持。因为有限的替代方法，只有几个函数可用。

注意: 当一个字符参数忽略，可以使用 `$config['charset']`。

## 依赖性

- [iconv](<http://php.net/iconv>) 扩展

注意: 这个依赖性是可选的，这个函数总是被声明。如果 iconv 不可用，他们将会回退到 non-mbstring 版本。

注意: 当提供字符设置时，必须通过 iconv 和它认识的格式来支持。

注意: 你再检查 mbstring 扩展时，可以使用 `MB_ENABLED` 常量。

## 函数参考

#### mb_strlen($str[, $encoding = NULL])

* 参数 字符串	$str: 输入字符串
* 参数 字符串	$encoding: 字符集
* 返回	输入字符串的字符数，失败返回 FALSE
* 返回类型	字符串

更多信息，可以参考 [PHP mb_strlen() 使用手册](http://php.net/mb_strlen)。

#### mb_strpos($haystack, $needle[, $offset = 0[, $encoding = NULL]])

* 参数 字符串	$haystack: 需要搜索的字符串
* 参数 字符串	$needle: 需要搜索部分字符串
* 参数 整数	$offset: 搜索偏移量
* 参数 字符串	$encoding: 字符集
* 返回	$needle 字符所在的位置，失败返回 FALSE
* 返回类型	混合

更多信息，可以参考 [PHP mb_strpos() 使用手册](http://php.net/mb_strpos)。

#### mb_substr($str, $start[, $length = NULL[, $encoding = NULL]])

* 参数 字符串	$str: 输入字符串
* 参数 整数	$start: 第一个字符的位置
* 参数 整数	$length: 字符的最大数量
* 参数 字符串	$encoding: 字符集
* 返回	返回从 $start 开始，长度为 $lengthPortion 的字符串，失败返回 FALSE。
* 返回类型	字符串

更多信息，可以参考 [PHP mb_substr() 使用手册](http://php.net/mb_substr)。

## 标准函数

这套兼容性函数提供一些标准的 PHP 函数支持，不过需要新版本的 PHP。

### 依赖性

- 无

### 函数参考

#### array\_column(array $array, $column_key[, $index_key = NULL])

* 参数 数组	$array: 取结果的源数组
* 参数 混合	$column_key: 取结果的列的 key
* 参数 混合	$index_key: 返回值的 key
* 返回	从多维数组中取回某列数组
* 返回类型	数组

更多信息，可以参考 [PHP array_column() 使用手册](http://php.net/array_column)。

#### array\_replace(array $array1[, ...])

* 参数 数组	$array1: 将要替换的数组
* 参数 array	...: 需要提前内容的数组
* 返回	修改后的数组
* 返回类型	数组

更多信息，可以参考 [PHP array_replace() 使用手册](http://php.net/array_replace)。

#### array\_replace\_recursive(array $array1[, ...])

* 参数 数组	$array1: 将要替换内容的数组
* 参数 array	...: 将要提取内容的数组
* 返回	修改后的数组
* 返回类型	数组

更多信息，可以参考 [PHP array_replace_recursive() 使用手册](http://php.net/array_replace_recursive)。

注意: 只有 PHP 原生函数可以检测无穷递归。除非你的 PHP 版本是 5.3+,否则你要消息使用。

#### hex2bin($data)

* 参数 数组	$data: 16 进制的数据
* 返回	参数的二级制表示
* 返回类型	字符串

更多信息，可以参考 [PHP hex2bin() 使用手册](http://php.net/hex2bin)。

#### quoted_printable_encode($str)

* 参数 字符串	$str: 输入字符串
* 返回	8bit-encoded 字符串
* 返回类型	字符串

更多信息，可以参考 [PHP quoted_printable_encode() 使用手册](http://php.net/quoted_printable_encode)。
