<h2><a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/nrf/samples/bluetooth/peripheral_uart/README.html">蓝牙：外围串口</a></h2>
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

<h3>FEM支持</h3>
<p>你可以增加支持对nRF21540前端模块（front-end module）在这个例程中通过使用以下的选项中的一个，依赖于你的硬件：【根据您的硬件，您可以使用以下选项之一向此示例添加对 nRF21540 前端模块的支持：】</p>
<ul>
  <li>为一个包含nRF21540 FEM的板子构建这个例程，例如<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/boards/arm/nrf21540dk_nrf52840/doc/index.html#nrf21540dk-nrf52840">nrf21540dk_nrf52840</a>。</li>
  <li>手工的（Manually）创建一个设备树覆盖（？overlay）文件，它描述了FEM如何与你设备的nRF5 SOC连接。参考<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/guides/dts/howtos.html#set-devicetree-overlays">Set devicetree overlays</a>了解添加覆盖文件的不同方法。</li>
  <li><p>提供nRF21540 FEM性能（capability）通过使用一个屏蔽（shield）【通过使用屏蔽提供 nRF21540 FEM 功能】，比如nRF21540 EK屏蔽，它是允许的在nRF连接SDK中。【例如 nRF Connect SDK 中提供的 nRF21540 EK 屏蔽。】在这个例子中，构建这个项目为了一个板级连接。。。【在这种情况下，使用 build 命令中包含的适当(appropriate)变量为连接到您正在使用的屏蔽的板构建项目。】这个变量指导构建系统去增加适当的设备树覆盖文件。例如，从命令行为附有nRF21540EK的nRF52833 DK构建这个实例，使用如下的命令在实例目录里：</p>
    <pre>west build -b nrf52833dk_nrf53833 -- -DSHIELD=nrf21540_ek</pre>
    <p>这个命令构建应用架构。参考<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/nrf/ug_radio_fem.html#ug-radio-fem-nrf21540-ek-programming">Programming nRF21540 EK</a>获取更多关于当你在使用一个有网络核的板子时如何编程的信息，例如nRF5340 DK。</p></li>
</ul>
<p>每个选项添加nRF21540 FEM的描述至设备树。【这些选项中的每一个都将 nRF21540 FEM 的描述添加到设备树中。】参考<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/nrf/ug_radio_fem.html#ug-radio-fem">Radio front-end module(FEM) support</a>获取更多关于nRF Connect SDK中的FEM信息。</p>
<p>去添加支持为另一个前端模块，添加各自的（respective）设备树文件包含板级设备树文件或设备树覆盖文件。【要添加对其他前端模块的支持，请将相应的设备树文件条目添加到板设备树文件或设备树覆盖文件中。】</p>

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
