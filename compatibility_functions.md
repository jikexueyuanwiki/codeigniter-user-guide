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

.. php:function:: password_get_info($hash)

	:param	string	$hash: Password hash
	:returns:	Information about the hashed password
	:rtype:	array

	For more information, please refer to the `PHP manual for
	password_get_info() <http://php.net/password_get_info>`_.

.. php:function:: password_hash($password, $algo[, $options = array()])

	:param	string	$password: Plain-text password
	:param	int	$algo: Hashing algorithm
	:param	array	$options: Hashing options
	:returns:	Hashed password or FALSE on failure
	:rtype:	string

	For more information, please refer to the `PHP manual for
	password_hash() <http://php.net/password_hash>`_.

	.. note:: Unless you provide your own (and valid) salt, this function
		has a further dependency on an available CSPRNG source. Each
		of the following would satisfy that:
		- ``mcrypt_create_iv()`` with ``MCRYPT_DEV_URANDOM``
		- ``openssl_random_pseudo_bytes()``
		- /dev/arandom
		- /dev/urandom

.. php:function:: password_needs_rehash()

	:param	string	$hash: Password hash
	:param	int	$algo: Hashing algorithm
	:param	array	$options: Hashing options
	:returns:	TRUE if the hash should be rehashed to match the given algorithm and options, FALSE otherwise
	:rtype:	bool

	For more information, please refer to the `PHP manual for
	password_needs_rehash() <http://php.net/password_needs_rehash>`_.

.. php:function:: password_verify($password, $hash)

	:param	string	$password: Plain-text password
	:param	string	$hash: Password hash
	:returns:	TRUE if the password matches the hash, FALSE if not
	:rtype:	bool

	For more information, please refer to the `PHP manual for
	password_verify() <http://php.net/password_verify>`_.

*********************
Hash (Message Digest)
*********************

This compatibility layer contains backports for the ``hash_equals()``
and ``hash_pbkdf2()`` functions, which otherwise require PHP 5.6 and/or
PHP 5.5 respectively.

Dependencies
============

- None

Function reference
==================

.. php:function:: hash_equals($known_string, $user_string)

	:param	string	$known_string: Known string
	:param	string	$user_string: User-supplied string
	:returns:	TRUE if the strings match, FALSE otherwise
	:rtype:	string

	For more information, please refer to the `PHP manual for
	hash_equals() <http://php.net/hash_equals>`_.

.. php:function:: hash_pbkdf2($algo, $password, $salt, $iterations[, $length = 0[, $raw_output = FALSE]])

	:param	string	$algo: Hashing algorithm
	:param	string	$password: Password
	:param	string	$salt: Hash salt
	:param	int	$iterations: Number of iterations to perform during derivation
	:param	int	$length: Output string length
	:param	bool	$raw_output: Whether to return raw binary data
	:returns:	Password-derived key or FALSE on failure
	:rtype:	string

	For more information, please refer to the `PHP manual for
	hash_pbkdf2() <http://php.net/hash_pbkdf2>`_.

****************
Multibyte String
****************

This set of compatibility functions offers limited support for PHP's
`Multibyte String extension <http://php.net/mbstring>`_. Because of
the limited alternative solutions, only a few functions are available.

.. note:: When a character set parameter is ommited,
	``$config['charset']`` will be used.

Dependencies
============

- `iconv <http://php.net/iconv>`_ extension

.. important:: This dependency is optional and these functions will
	always be declared. If iconv is not available, they WILL
	fall-back to their non-mbstring versions.

.. important:: Where a character set is supplied, it must be
	supported by iconv and in a format that it recognizes.

.. note:: For you own dependency check on the actual mbstring
	extension, use the ``MB_ENABLED`` constant.

Function reference
==================

.. php:function:: mb_strlen($str[, $encoding = NULL])

	:param	string	$str: Input string
	:param	string	$encoding: Character set
	:returns:	Number of characters in the input string or FALSE on failure
	:rtype:	string

	For more information, please refer to the `PHP manual for
	mb_strlen() <http://php.net/mb_strlen>`_.

.. php:function:: mb_strpos($haystack, $needle[, $offset = 0[, $encoding = NULL]])

	:param	string	$haystack: String to search in
	:param	string	$needle: Part of string to search for
	:param	int	$offset: Search offset
	:param	string	$encoding: Character set
	:returns:	Numeric character position of where $needle was found or FALSE if not found
	:rtype:	mixed

	For more information, please refer to the `PHP manual for
	mb_strpos() <http://php.net/mb_strpos>`_.

.. php:function:: mb_substr($str, $start[, $length = NULL[, $encoding = NULL]])

	:param	string	$str: Input string
	:param	int	$start: Position of first character
	:param	int	$length: Maximum number of characters
	:param	string	$encoding: Character set
	:returns:	Portion of $str specified by $start and $length or FALSE on failure
	:rtype:	string

	For more information, please refer to the `PHP manual for
	mb_substr() <http://php.net/mb_substr>`_.

******************
Standard Functions
******************

This set of compatibility functions offers support for a few
standard functions in PHP that otherwise require a newer PHP version.

Dependencies
============

- None

Function reference
==================

.. php:function:: array_column(array $array, $column_key[, $index_key = NULL])

	:param	array	$array: Array to fetch results from
	:param	mixed	$column_key: Key of the column to return values from
	:param	mixed	$index_key: Key to use for the returned values
	:returns:	An array of values representing a single column from the input array
	:rtype:	array

	For more information, please refer to the `PHP manual for
	array_column() <http://php.net/array_column>`_.

.. php:function:: array_replace(array $array1[, ...])

	:param	array	$array1: Array in which to replace elements
	:param	array	...: Array (or multiple ones) from which to extract elements
	:returns:	Modified array
	:rtype:	array

	For more information, please refer to the `PHP manual for
	array_replace() <http://php.net/array_replace>`_.

.. php:function:: array_replace_recursive(array $array1[, ...])

	:param	array	$array1: Array in which to replace elements
	:param	array	...: Array (or multiple ones) from which to extract elements
	:returns:	Modified array
	:rtype:	array

	For more information, please refer to the `PHP manual for
	array_replace_recursive() <http://php.net/array_replace_recursive>`_.

	.. important:: Only PHP's native function can detect endless recursion.
		Unless you are running PHP 5.3+, be careful with references!

.. php:function:: hex2bin($data)

	:param	array	$data: Hexadecimal representation of data
	:returns:	Binary representation of the given data
	:rtype:	string

	For more information, please refer to the `PHP manual for hex2bin()
	<http://php.net/hex2bin>`_.

.. php:function:: quoted_printable_encode($str)

	:param	string	$str: Input string
	:returns:	8bit-encoded string
	:rtype:	string

	For more information, please refer to the `PHP manual for
	quoted_printable_encode() <http://php.net/quoted_printable_encode>`_.
