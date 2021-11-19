# 模板引擎扩展


## 安装和配置
修改项目下的composer.json文件，并添加：  
```
    "phalapi/template":"dev-master"
```

然后执行```composer update```。  

## 注册
在/path/to/phalapi/config/di.php文件中，注册：  
```php

$di->tpl = function() {
	return new \PhalApi\Template\Lite(array(
        'view_path' => '模板路径',
        'cache_path'=> '模板编译缓存路径'
    ));
};
```

## 使用

```php
// 模板变量赋值
$di->tpl->assign(array('name' => 'think'));
// 读取模板文件渲染输出
$di->tpl->fetch('index');
// 完整模板文件渲染
$di->tpl->fetch('./template/test.php');
// 渲染内容输出
$di->tpl->display($content);
```

配置参数（详见phalapi/template/src/Lite.php）
```php
array(
    'view_path'          => '', // 模板路径
    'view_suffix'        => 'html', // 默认模板文件后缀
    'view_depr'          => DIRECTORY_SEPARATOR,
    'cache_path'         => '',
    'cache_suffix'       => 'php', // 默认模板缓存后缀
    'tpl_deny_func_list' => 'echo,exit', // 模板引擎禁用函数
    'tpl_deny_php'       => false, // 默认模板引擎是否禁用PHP原生代码
    'tpl_begin'          => '{', // 模板引擎普通标签开始标记
    'tpl_end'            => '}', // 模板引擎普通标签结束标记
    'strip_space'        => false, // 是否去除模板文件里面的html空格与换行
    'tpl_cache'          => true, // 是否开启模板编译缓存,设为false则每次都会重新编译
    'compile_type'       => 'file', // 模板编译类型
    'cache_prefix'       => '', // 模板缓存前缀标识，可以动态改变
    'cache_time'         => 0, // 模板缓存有效期 0 为永久，(以数字为值，单位:秒)
    'layout_on'          => false, // 布局模板开关
    'layout_name'        => 'layout', // 布局模板入口文件
    'layout_item'        => '{__CONTENT__}', // 布局模板的内容替换标识
    'taglib_begin'       => '{', // 标签库标签开始标记
    'taglib_end'         => '}', // 标签库标签结束标记
    'taglib_load'        => true, // 是否使用内置标签库之外的其它标签库，默认自动检测
    'taglib_build_in'    => 'cx', // 内置标签库名称(标签使用不必指定标签库名称),以逗号分隔 注意解析顺序
    'taglib_pre_load'    => '', // 需要额外加载的标签库(须指定标签库名称)，多个以逗号分隔
    'display_cache'      => false, // 模板渲染缓存
    'cache_id'           => '', // 模板缓存ID
    'tpl_replace_string' => array(),
    'tpl_var_identify'   => 'array', // .语法变量识别，array|object|'', 为空时自动识别
    'default_filter'     => 'htmlentities', // 默认过滤方法 用于普通标签输出
);
```
##基础方法（详见phalapi/template/src/SDK/Cx.php）

```php
//api.php
$output = array();
$output['title'] = '标题';
$output['list'] = array(
    array('title' => '标题1'),
    array('title' => '标题2')
);
$di->tpl->assign($output);      
$di->tpl->fetch('index');

//index.html
<h1 class="title">{$title}</h1>

<ul class="list-unstyled">
    {foreach name="list" id="vo" key="k" index="i"}
    <li>{$vo.title}</li>
    {/foreach}
</ul>
```

##总结

希望此拓展能够给大家带来方便以及实用,拓展是从ThinkPHP分离出来的模板引擎，支持XML标签和普通标签的模板解析，编译型模板引擎，支持动态缓存。

注:笔者能力有限有说的不对的地方希望大家能够指出,也希望多多交流!

笔者QQ:49078111
