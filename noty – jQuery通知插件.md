[noty – jQuery通知插件](https://www.cnblogs.com/lwme/p/noty-jquery-notification-plugin.html)

https://www.cnblogs.com/lwme/p/noty-jquery-notification-plugin.html

noty是一个jQuery的通知(信息提示)插件，灵活轻便，是一个非常棒的用于替代传统提示对话框的插件。

当前最新版本为2.1.0: 从 https://github.com/needim/noty  可以获取最新源代码。
[toc]
### 布局样式
支持的提示样式，每种样式都有预定义好的css样式：

    alert: 默认的提示样式
    success: 成功
    error: 错误
    warning: 警告
    information: 信息

支持的布局位置：

    top: 顶部,长条状
    topLeft/topCenter/topRight: 顶部的左/中/右位置, 短条状
    center/centerLeft/centerRight: 正中/中左/中右, 短条状
    bottomLeft/bottomCenter/bottomRight: 底部左/中/右位置, 短条状
    bottom: 底部,长条状

除上以上布局方式之外,还有一种用于自定义布局的inline方式,需要引入layouts/inline.js

### 安装脚本
从https://github.com/needim/noty 下载最新源码,然后引入相应脚本文件:
```
<!--jQuery文件-->
<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.7/jquery.min.js"></script>
<!--noty主文件-->
<script type="text/javascript" src="js/noty/jquery.noty.js"></script>
<!--noty提示信息位置的文件, 需要哪些位置就引入对应的脚本,这里为center,可以添加多个布局文件-->
<script type="text/javascript" src="js/noty/layouts/center.js"></script>
<!--noty主题样式文件,-->
<script type="text/javascript" src="js/noty/themes/default.js"></script>
```
如果你使用的是jQuery 1.6以下版本, 那么还需要引入promise.js文件。

### 使用方法
一般情况下，直接使用noty(options)全局函数来创建提示信息即可:
```
noty({text: "noty - jQuery 通知插件 - 囧月 - lwme.cnblogs.com", layout: "center", timeout: 5000});
```

以上代码将创建一个在屏幕中间显示的提示信息,并在5秒后自动关闭。

注意:如果没有对应布局的js文件将不会被正常呈现。

#### 默认选项
noty有以下可供设置的选项:
```
$.noty.defaults = {
    layout: 'top',  // 默认布局
    theme: 'defaultTheme', // 默认主题
    type: 'alert', // 默认类型
    text: '', //默认文本
    dismissQueue: true, // 是否添加到队列
    template: '<div class="noty_message"><span class="noty_text"></span><div class="noty_close"></div></div>',  // 消息默认模板
    animation: { //默认的显示及关闭动画
        open: {height: 'toggle'},
        close: {height: 'toggle'},
        easing: 'swing',
        speed: 500 // opening & closing animation speed
    },
    timeout: false, // 自动关闭时间,默认不会自动关闭
    force: false, // 添加到队列开始处
    modal: false, // 遮罩
    maxVisible: 5, // 一个队列的消息最大可见数量, 即一个队列中同一时间最多显示的数量
    closeWith: ['click'], // ['click', 'button', 'hover'] 关闭的事件,默认点击消息关闭
    callback: { // 回调函数
        onShow: function() {}, // 显示之前
        afterShow: function() {}, // 显示之后
        onClose: function() {}, // 关闭之前
        afterClose: function() {} // 关闭之后
    },
    buttons: false // 按钮,用于在弹出的消息框中显示按钮
};
```

#### 自定义容器
通过noty(options)函数创建的提示信息默认被添加到body上, noty支持在自定义容器中显示提示的方式:
```
$('.custom_container').noty({text: "noty - jQuery 通知插件 - 囧月 - lwme.cnblogs.com"});
```

#### 按钮及确认对话框
可以像这样设置提示信息上的按钮：
```
noty({
  text: '你要继续吗?',
  buttons: [
    {addClass: 'btn btn-primary', text: '确定', onClick: function($noty) {
        // this = button element 也就是当前的按钮
        // $noty = $noty element 也就是当前这个提示信息对象
        $noty.close();
        noty({text: '你点击了确定按钮', type: 'success'});
      }
    },
    {addClass: 'btn btn-danger', text: '取消', onClick: function($noty) {
        $noty.close();
        noty({text: '你点击了取消按钮', type: 'error'});
      }
    }
  ]
});
```
官方DEMO中的确认提示对话框也是这么创建的。

#### API
主要有以下api可用：

    $.noty.get(id): 通过id获取noty对象, 一般用不到
    $.noty.close(id): 关闭一个noty提示
    $.noty.clearQueue(): 清空队列中的noty
    $.noty.closeAll(): 关闭所有noty提示
    $.noty.setText(id, text): 更新noty提示信息的文本内容
    $.noty.setType(id, type): 更新noty提示信息的类型

一般情况下通过实例变量来访问它的属性/方法:
```
var n = noty({text: "noty - jQuery 通知插件 - 囧月 - lwme.cnblogs.com"});
n.setText("囧月 - lwme.cnblogs.com"); // 更新内容
n.setTimeout(10000); // 设置超时时间
n.setType("error"); // 更新提示类型
n.close();
// 此外, noty还有几个属性用于查看它的状态
n.closed // 是否已关闭
n.showing // 是否正在显示
n.shown // 是否已显示
```
而$.noty空间最常用的就是clearQueue()和closeAll()方法了，其他几个方法一般不被使用，主要是因为noty的id是随机生成的。

除了文档里公开的，noty还有一些没在文档里公开的属性：

    $.noty.queue 消息队列, Array
    $.noty.layouts 可以获取当前加载的布局, Object
    $.noty.themes 获取可用的主题, Object
    $.noty.store 获取当前显示在页面中的noty对象,然后可以通过api进行操作, Array
    $.noty.returns 获取noty(options)函数返回的值, 默认为object返回noty对象,可以改成其他值返回noty对象的id

此外,noty还提供了方法用来替代window.alert函数:

    $.noty.consumeAlert 替代window.alert
    $.noty.stopConsumeAlert 取消替代

 

noty整体设置比较简单，没有过多的参数设置，使用起来也很方便，特别是默认不用按钮来让用户点确定这点特别值得称赞，体验相当不错。

更多的例子请看官方：http://needim.github.io/noty/

--EOF--