# 创建适配器

## 适配器文件夹和文件结构

适配器目录和文件结构如下:

-  /application/libraries/Driver_name

   -  Driver_name.php
   -  drivers

      -  Driver_name_subclass_1.php
      -  Driver_name_subclass_2.php
      -  Driver_name_subclass_3.php

注意: 为了和文件名大小写敏感的文件系统兼容，Driver_name 必须用 `ucfirst()` 处理一下

注意: 从适配器架构开说，子类不会扩展并且不会继承主驱动器的属性或方法。
