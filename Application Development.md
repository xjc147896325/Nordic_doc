<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/application/index.html#important-build-vars">源文件</a>
<hr>

<h2></h2>
<h2></h2>
<h2></h2>
<h2></h2>
<h2></h2>
<h2></h2>
<h2>总览</h2>
<p>zephyr的系统构建基于CMake</p>
<p>这个构建系统以应用为核心，并且需要基于zephyr的应用去初始化内核的源码树。(application-centric——以应用为核心的）应用构建<strike>包含控制配置和创建程序的应用和内核自己</strike>控制应用程序和Zephyr本身的配置和构建过程，编译他们为一个二进制文件。</p>
<p>Zephyr的基本目录包含Zephyr自己的源码，它的内核配置选项，和它自己的构建定义。</p>
<p>在应用文件目录中的文件连接了应用和Zephyr。这个目录包含所有<strike>的以应用为核心的</strike>特定于应用程序的文件，例如配置选项和源码。</p>
<p><strike>一个应用最简单的表格</strike>最简单形式的应用程序有着下面的内容：</p>
<pre>
home/app
├── CMakeLists.txt
├── prj.conf
└── src
    └── main.c
</pre>
<p>这些内容是：</p>
<p><b>CMakeList.txt：</b>这个文件告诉构建系统哪里去找到其他的应用文件，并且<strike>连接Zephyr的CMake构建系统的应用目录</strike>将应用程序目录和Zephyr的CMake构建系统链接起来。这个链接提供由Zephyr的构建系统支持的功能，例如特定板子的内核配置文件，在真实的或者仿真的硬件环境中运行和调试编译过的二进制文件<strike>在真实的或者仿真的硬件环境</strike>的能力，等等。</p>
<p><b>Kernel configuration files：</b><strike>一个典型的应用提供了一个指定特定应用的只为了一个或多个内核配置选项的Kconfig配置文件（经常被命名为pri.conf）</strike>应用程序通常提供一个 Kconfig 配置文件（通常称为 prj.conf），它为一个或多个内核配置选项指定特定于应用程序的值。这些应用设置与特定板子的设置合并去生成一个内核配置。</p>
<p>参考<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/application/index.html#application-kconfig">内核配置</a>去了解更多信息。</p>
<p><b>Applications source code files：</b>应用程序通常<strike>一个典型的应用</strike>提供了一个或多个特定的应用文件，用C或者汇编写的。这些文件经常被放在一个被称为src的子目录（sub-directory）</p>
<p>一旦一个应用被定义了，为了构建一个项目放置在要托管的目录，你可以使用CMake去创建它。【定义应用程序后，您可以使用 CMake 创建项目文件，以便从要托管这些文件的目录中构建它。】这被称作<b>构建目录</b>。应用程序构建的人工产物经常在一个构建目录中被生成，Zephyr不支持“in tree”构建。</p>
<p>接下来的<strike>块</strike>部分描述了如何创建、生成并且运行Zephyr应用，<strike>伴有</strike>然后是（be followed by）更多的详细资料。</p>
<h2>源码的结构树</h2>
<p>理解Zephyr源码树对于定位代码和具体的Zephyr功能的结合是很有帮助的。【了解 Zephyr 源代码树有助于定位与特定 Zephyr 功能相关联的代码。】</p>
<p>有一些重要的文件在树的顶层。</p>
<ul>
    <h4><li>
        CMakeLists.txt</h4>
        <p>CMake构建系统的最顶层的文件，包含了<strike>一大堆</strike>很多创建Zephyr所需要的逻辑。</p>
    </li>
    <h4><li>
        Kconfig</h4>
        <p>Kconfig顶层文件，<strike>在顶层的目录的Kconfig.zephyr文件一样可以找到</strike>它指的是在顶级目录中也找到的文件 Kconfig.zephyr。</p>
        <p>参考<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/guides/build/index.html#kconfig">Fconfig手册</a>获取更详细的文档。</p>
    </li>
    <h4><li>
        west.yml</h4>
        <p></p>
    </li>
    <h4><li>
        CMakeLists.txt</h4>
        <p><a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/guides/west/index.html#west">West（Zephyr 的元工具）</a><strike>截货单</strike>清单，列举了被管理在west命令行工具的额外仓库。</p>
    </li>
</ul>
<p>Zephyr源码树也包含如下顶部目录，每个可能有着一个或者多个额外的没有在这提及【描述】的子目录</p>
<ul>
    <h4><li>
        arch</h4>
    <p>特<strike>别</strike>定的内核架构和片上系统（SOC）代码，每个支持的架构（如X86、ARM）有着自己的子目录，它包含了如下额外的子目录：</p>
    <ul>
        <li>特<strike>别</strike>定的内核架构源文件。</li>
        <li>特<strike>别</strike>定的架构的私有API的头文件。</li>
    </ul>
    </li>
    <h4><li>
        soc</h4>
        <p>与片上系统有关的代码和配置文件。</p>
    </li>
    <h4><li>
        boards</h4>
        <p>板子相关的代码和配置文件。</p>
    </li>
    <h4><li>
        doc</h4>
        <p><strike>zephyr的源码和工具的技术文档，经常在<a href="https://docs.zephyrproject.org">https://docs.zephyrproject.org</a>页面里被生成。</strike></p>
        <p>Zephyr 技术文档源文件和用于生成 <a href="https://docs.zephyrproject.org">https://docs.zephyrproject.org</a> 网络内容的工具。</p>
    </li>
    <h4><li>
        drivers</h4>
        <p>设备驱动代码。</p>
    </li>
    <h4><li>
        dts</h4>
        <p>设备树源文件用于描述不可发现的基于特定板子的硬件细节</p>
    </li>
    <h4><li>
        include</h4>
        <p>所有公共API的头文件，<strike>其他的定义在lib中</strike>lib下定义的文件除外。</p>
    </li>
    <h4><li>
        kernel</h4>
        <p><strike>独立架构</strike>与体系架构无关的kernel代码</p>
    </li>
    <h4><li>
        CMakeLists.txt</h4>
        <p></p>
    </li>
    <h4><li>
        CMakeLists.txt</h4>
        <p></p>
    </li>
    <h4><li>
        CMakeLists.txt</h4>
        <p></p>
    </li>
    <h4><li>
        CMakeLists.txt</h4>
        <p></p>
    </li>
    <h4><li>
        CMakeLists.txt</h4>
        <p></p>
    </li>
    <h4><li>
        CMakeLists.txt</h4>
        <p></p>
    </li>
    <h4><li>
        CMakeLists.txt</h4>
        <p></p>
    </li>
    <h4><li>
        CMakeLists.txt</h4>
        <p></p>
    </li>
    <h4><li>
        CMakeLists.txt</h4>
        <p></p>
    </li>
    <h4><li>
        CMakeLists.txt</h4>
        <p></p>
    </li>
    <h4><li>
        CMakeLists.txt</h4>
        <p></p>
    </li>
    <h4><li>
        CMakeLists.txt</h4>
        <p></p>
    </li>
    <h4><li>
        CMakeLists.txt</h4>
        <p></p>
    </li>
    <h4><li>
        CMakeLists.txt</h4>
        <p></p>
    </li>
</ul>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
<p></p>
