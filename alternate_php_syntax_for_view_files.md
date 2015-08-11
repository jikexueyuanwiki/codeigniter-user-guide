# 视图文件的 PHP 替代语法

如果你不使用 CodeIgniter 的模板语法，你可以在视图文件中使用原始的 PHP 代码。要使这些文件里的 PHP 代码最小化，并让他们容易辨认，建议你使用 PHP 替代语法，来控制结构和短标签 echo 语句。如果你不熟悉这个语法，下面内容将会让你消灭大括号和 "echo" 语句。

## 自动短标签支持

注意: 如果你发现本页描述语法在你的服务器上不能工作，可能是因为你的 PHP ini 文件禁用了 "short tags"。CodeIgniter 将会选择性的重写，运行你使用语法，及时你的服务器不支持。可以在 *config/config.php* 文件中打开这个特性。

请注意，如果你使用这个特性，如果 PHP 错误在你的视图文件中出现，错误消息和行数不会准确的出现。相反，所有的错误将会展示为 `eval()` 错误。

## 替代 Echo

echo，或者打印一个变量，可以这么写：

	<?php echo $variable; ?>

使用替换语法，你可以这么写：

	<?=$variable?>

## 替代控制结构

控制结构，像 if，for，foreach，和 while 也可以写成简化的形式。这里是一个用 foreach 的例子：

	<ul>

	<?php foreach ($todo as $item): ?>

		<li><?=$item?></li>

	<?php endforeach; ?>

	</ul>

注意，这里没有大括号，它被 `endforeach` 替换。每个上述的控制结构拥有相同结束语法：`endif`, `endfor`, `endforeach`, 和 `endwhile`.

同时也需要注意，每个结构以后不用分号（除了最后一个），用冒号，这很重要！

这有另一个例子，使用 `if`/`elseif`/`else`. 注意冒号 ::

	<?php if ($username === 'sally'): ?>

		<h3>Hi Sally</h3>

	<?php elseif ($username === 'joe'): ?>

		<h3>Hi Joe</h3>

	<?php else: ?>

		<h3>Hi unknown user</h3>

	<?php endif; ?>
