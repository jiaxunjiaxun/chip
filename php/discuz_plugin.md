# Discuz插件的编写

初始的一些了解插件工作原理的内容可以从Discuz的社区获得帮助（见附录）。

- 确定插件是独立执行还是与Discuz协同执行，独立执行的用plugin_name.inc.php，协同执行的用plugin_name.class.php，命名可以不同，主要是管理方便。

- 特别要说一下很多插件里出现的template目录，这下面的模板以一种特殊的方式展现页面（**eval** 函数），主要是为了将插件的业务逻辑和视图部分分开。阅读官方插件的写法，有助于理解这一点的优势，需要注意的是在插件中的加载时机，是在父类的构造函数中。

- 数据库的操作尽量出现在table目录下，这可以看作是数据访问层，也是对原始数据的一种封装。不让SQL出现在顺序执行的源代码中是个不错的习惯，也便于维护。命名规则要存从Discuz的规则，不然会有很多加载的问题。

# 开发提示

## 独立入口（[plugin_file_name].inc.php）

这类入口通过plugin.php?id=[plugin_id]:[plugin_file_name]的方式访问，可以认为是一个通用的入口，这里的扩展是简单的面向过程。

~~~ php
<?php
defined('IN_DISCUZ') or die('Access Denied');
// 剩下的按照需求完成
// 这里可以看到很多Discuz的环境变量
~~~

## 协同入口（[plugin_file_name].class.php）

这类入口通过Discuz的钩子方式执行，需要注意的就是template文件夹下面的视图部分一般会在这里调用，这是一种灵活性，当然我对这种设计表示担忧。

~~~ php
<?php
// 以下文档改编自官方文档
// 全局嵌入点类（必须存在）
class plugin_identifier {

    public function __construct() {
        global $_G;
        // 引入视图函数
        include_once template('plugin_identifier:template_name');
        ...
    }

    // 这里需要查看官方文档关于全局钩子的说明
    public function global_footer() {
        ......
        return ...;
    }

    ......
}

// 脚本嵌入点类
class plugin_identifier_CURSCRIPT extends plugin_identifier {

    public funcion __construct() {
    }

    // 这里需要查看官方文档关于特定脚本钩子的说明
    // 注册表单显示结束
    public function register_input_output() {
        ......
        return ...;
    }

    // 注册表单提交，并不是在模板上执行的，而是在runhook函数中执行的，先于模板渲染
    public function register_logging_method() {
        ......
        return ...;
    }

    ......
}
~~~

## 安装、卸载与更新

install.php用于插件安全时执行，uninstall.php插件卸载时执行，upgrade.php插件更新时执行。

~~~ php
<?php
defined('IN_DISCUZ') or die('Access Denied');

$sql = <<<EOF
...
EOF;

// 公共函数库
runquery( $sql );
~~~

## table文件夹下的数据访问层

分开写的好处多多，不管在哪种语言都一样，我就不废话了。

~~~ php
<?php
// file: table_[table_name_without_prefix].php
class table_[table_name_without_prefix] extends discuz_table {

    public function __construct() {
        $this->_table = 'table_name_without_prefix';
        $this->_pk = 'primary_key';
        parent::__construct();
    }

    public function data_access_method() {
        // 参考Discuz的数据库类
        DB::fetch...
    }
}

// usage: other file
C::t('#plugin_identifier#table_name_without_prefix')->data_access_method(...);
~~~

## template文件夹下的数据展现层

template的数据展现层设计个人感觉是一种不太好的方式，但受限于Discuz系统。

~~~ php
// 注意：这是一个htm文件，命名没有约束，所以要注意规范。
<!--{eval
function tpl_hook_id_output() {
    global $_G;
}-->
<!--{block return}-->
HTML、CSS、Javascript混合编写问题很多，无须赘述。
<!--{/block}-->
<!--{eval return $return; }-->
<!--{eval
}
}-->
~~~

# 附录

## 参考资源
- [FAQ](http://faq.comsenz.com/library/)
- [开放平台](http://open.discuz.net/)
- [第三方](http://discuzt.cr180.com/)