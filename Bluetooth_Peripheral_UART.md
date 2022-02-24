<h2>蓝牙：外围串口</h2>
<p>这个外围串口例程示范（demonstrate）了如何使用Nordic串口服务（<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/nrf/libraries/bluetooth_services/services/nus.html#nus-service-readme">Nordic UART Service(NUS)</a>）。它使用NUS去发送返回数据并且前往UART连接和BLE连接之间，仿真一个串行端口覆盖BLE。【它使用 NUS 服务在 UART 连接和蓝牙® LE 连接之间来回发送数据，模拟通过蓝牙 LE 的串行端口。】</p>
<hr>

<h3>总览</h3>
<p>当连接建立，这个例程发送任何从UART0的RX接收到的数据至蓝牙LE单元。在Nordic 微电子的开发套件上，UART0外设通常会通过SEGGER芯片连接到USB CDC虚拟串口。</p>
<p>任何从BLE单元发送的数据被发送至UART0外设的TX引脚。</p>
<pre>NOTE：
Thingy：53 使用USB CDC ACM类的第二个实例（instance）以替代UART0，因为它没有内置的SEGGER芯片用于门控UART0。</pre>

<h3>调试</h3>
<p>在这个例程中，UART控制台（console）通过NUS服务发送和读取数据。调试信息不显示在UART控制台。取而代之，他们被RTT 记录器（logger）打印</p>
<p>如果你想要浏览调试信息，跟着在<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/nrf/gs_testing.html#testing-rtt-connect">Connecting via RTT</a>中的教程/步骤（procedure）</p>
<pre>NOTE：
在Thingy：53的调试记录/日志通过USB CDC ACM类串行端口提供，代替使用RTT。</pre>
<hr>

<h3></h3>
<p></p>
<ul>
  <li></li>
  <li></li>
  <li><p></p>
    <pre></pre>
    <p></p></li>
</ul>
<p></p>
<p></p>

<h3></h3>
<p></p>
<p></p>

<h3></h3>
<p></p>
<p></p>
<hr>

<h3></h3>
<p></p>
<table>
  <!-- daixu -->
