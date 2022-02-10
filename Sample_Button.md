<h1>botton</h1>
<hr>

<h2>Overview</h2>
<p>一个简单的按键demo，示例使用GPIO中断作为输入。这个例子向控制台（console）打印信息，当按键每次被按下时。</p>
<hr>

<h2>Requirements</h2>
<p>板子的硬件必须通过GPIO引脚连接一个按键。这被称做为“用户按键”在很多zephyr支持的板子中。</p>
<p>按键必须通过<b>sw0</b>的设备树别名（alias）被配置，常在BOARD.rts的文件中。你将会看到这个错误如果你尝试去构建这个例子为一个不支持的板子：</p>
<pre>Unsupported board: sw0 devicetree alias is not defined</pre>
<p>你可能看到额外的构建err如果<b>sw0</b>别名已经存在但没有被适当的（properly）定义。</p>
<p>这个例子额外的支持一个可选的<b>led0</b>设备树别名。这个是和Blinky中同样的别名。如果他被提供了，那个LED将会打开当按键被按下时，然后让释放时熄灭。</p>
<h3>Devicetree details</h3>
<p>这一节提供了更多的设备树配置细节。</p>
<p>这是一个最小的支持这个例子的设备树片段。它只包含一个<b>sw0</b>别名；可选的<b>led0</b>别名被简单的（simplicity）排除在外（is left out）。</p>
<pre>/ {
     aliases {
             sw0 = &button0;
     };

     soc {
             gpio0: gpio@0 {
                     status = "okay";
                     gpio-controller;
                     #gpio-cells = (2);
                     /* ... */
             };
     };

     buttons {
             compatible = "gpio-keys";
             button0: button_0 {
                     gpios = ( &gpio0 PIN FLAGS );
                     label = "User button";
             };
             /* ... other buttons ... */
     };
};</pre>
<p>如展示的，<b>sw0</b>设备树别名必须指出一个与“gpio-keys”兼容（compatible）的节点子结点。</p>
<p>上述的情况（above situation）是针对常见情况：</p>
<ul>
    <li>
        <p><b>gpio0</b>是一个参考/引用GPIO控制器的示例节点标签。</p>
    </li>
    <li>
        <p><b>PIN</b>应该是一个引脚号，像<b>8</b>或者<b>0</b></p>
    </li>
    <li>
        <p><b>FLAGS</b>应该是一个应用于按键的用逻辑或连接的GPIO配置标志（GPIO configuration flags)（meant to —— 为了），比如<b>(GPIO_PULL_UP | GPIO_ACTIVE_LOW)</b></p>
    </li>
</ul>
<p>这个假设了（assume）常见情况，其中<b>#gpio-cells = (2)</b>在<b>gpio0</b>节点里，并且GPIO控制器的设备树绑定了“pin”和“flags”这两个单元的名字，例如：</p>
<pre>gpio-cells:
  - pin
  - flags</pre>
<p>这个示例需要一个<b>pin</b>单元在<b>gpios</b>属性（property）里。<b>flags</b>单元是可选的，然而这个示例依旧可以工作如果GPIO单元不包含<b>flags</b>。</p>
<hr>

<h2>Building and Running</h2>
<p>这个示例可以对多数板子构建，在这个例子中我们将会为nuckeo_f103rb板子构建它。</p>
<pre>west build -b nucleo_f103rb samples/basic/button</pre>
<p>在启动后，这个程序查找（look up）一个预定义的GPIO设备，并且配置pin为输入模式，允许中断产生于下降沿。在每次主循环的迭代（iteration）中，GPIO线的状态是被监控和向串口控制台（console）。当输入按键被按下时，中断处理程序（handler——处理程序）将会打印一个信息关于这个事件以及（along with——伴随着）他的时间戳（timestamp）。</p>
<hr>
