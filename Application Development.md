<p><a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/application/index.html#important-build-vars">源文件</a></p>
<p>【】中的文字是谷歌翻得。</p>
<p>关于overlay file<a href="https://www.jianshu.com/p/ad19a76cac0c">简书的解释</a>，看起来是和Linux挂钩的，盲区，麻烦查缺补漏。</p>

<hr>

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
        lib</h4>
        <p>库代码，包含最小标准C库。</p>
    </li>
    <h4><li>
        misc</h4>
        <p>各种各样的/杂的（miscellaneous）代码，那些不属于任何其他顶层文件夹的。</p>
    </li>
    <h4><li>
        samples</h4>
        <p>例程应用用于证明如何使用Zephyr的功能。【演示 Zephyr 功能使用的示例应用程序。】</p>
    </li>
    <h4><li>
        scripts</h4>
        <p>各种程序和其他文件被使用于构建与测试Zephyr应用</p>
    </li>
    <h4><li>
        cmake</h4>
        <p>构建Zephyr需要的额外的【其他的】构建脚本（script）</p>
    </li>
    <h4><li>
        subsys</h4>
        <p>Zephyr的子系统，包括：</p>
        <ul>
            <li>USB设备栈代码。</li>
            <li>网络<strike>工作相关</strike>代码，包括蓝牙协议栈与网络<strike>工作</strike>协议栈。</li>
            <li>文件系统代码。</li>
            <li>蓝牙主机和控制器。</li>
        </ul>
    </li>
    <h4><li>
        test</h4>
        <p>Zephyr功能的测试代码和基准测试。</p>
    </li>
    <h4><li>
        share</h4>
        <p>额外的架构无关的【独立的】数据，目前包含Zephyr的CMake包。</p>
    </li>
</ul>
<h2>独立的（standalone）例程【示例独立应用程序】</h2>
<p>一个独立的包含它自己的Git文件夹的参考应用可以在<a href="https://github.com/zephyrproject-rtos/example-application">例程</a>文件夹中找到。它可以被用作如何使用<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/guides/west/workspaces.html#west-t2">T2星形拓扑（T2 star topology）</a>去构建基于Zephyr的树外（out-of-tree）应用的参考。<strike>他也演示了使用普通的树外功能的应用</strike>它还演示了应用程序中常用功能的树外使用，例如：</p>
<ul>
    <li>用户板子【定制板】</li>    
    <li>用户绑定设备树【自定义设备树绑定】</li>    
    <li>用户驱动【自定义驱动程序】</li>    
    <li>持续集成（Continuous Integration——CI）设置（setup）</li>    
</ul>
<h2>创建一个应用</h2>
<p>按照以下步骤去创建一个新的应用文件夹。（<strike>参考<a href="https://github.com/zephyrproject-rtos/example-application">例程</a>在它自己的Git库或者<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/samples/index.html#samples-and-demos">参考和测试版</a>为了已存在的应用被提供作为Zephyr的一部分，参考独立的应用存储库。</strike>（请参阅<a href="https://github.com/zephyrproject-rtos/example-application">示例应用程序</a>存储库以获取其自己的 Git 存储库中的参考独立应用程序，或参阅作为 Zephyr 一部分提供的现有应用程序的<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/samples/index.html#samples-and-demos">示例和演示</a>。）</p>
<ol>
    <li>
        <p>创建一个新的应用目录在你的工作站电脑，在Zephyr基础目录之外。通常你需要在你用户的<strike>根</strike>主目录的某个位置创建它。</p>
        <p>例如，在Unix的shell或者Windows的cmd.exe提示符下，导航到要创建应用程序的位置，然后键入：</p>
        <pre>mkdir app</pre>
        <p><b>警告：</b></p>
        <p>构建Zephyr或者创建一个应用程序在<strike>一个无论在哪个路径</strike>路径上任意位置中有着空格的文件夹是不被支持的。因此<pre>C:\Users\YourName\app</pre>这种Windows路径是可以的，但是<pre>C:\Users\Your Name\app</pre>是不行的</p>
    </li>
    <li>
        <p>推荐所有的应用程序的源码放置在一个叫做src的子目录下。这使得更容易区别于项目文件和源码。</p>
        <p>继续先前的例子，键入：</p>
        <pre>cd app
mkdir src</pre>
    </li>
    <li>
        <p>把你的源码放在src子目录下。在这个例程中，我们将会假定你创建了一个叫src/main.c的文件。</p>
    </li>
    <li>
        <p>创建一个叫CMakelist.txt的文件在app目录下，内容如下：</p>
        <pre># Find Zephyr. This also loads Zephyr's build system.
cmake_minimum_required(VERSION 3.13.1)
find_package(Zephyr)
project(my_zephyr_app)
# Add your source file to the "app" target. This must come after
# find_package(Zephyr) which defines the target.
target_sources(app PRIVATE src/main.c)</pre>
        <p>find_package(Zephyr)设置最低的CMake版本并且<strike>进</strike>引入Zephyr的构建系统，<strike>那个</strike>这会创建一个叫app的Cmake目标（参考<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/guides/zephyr_cmake_package.html#cmake-pkg">Zephyr Cmake包</a>）。<strike>你如何引入他们在构建中向这个目标添加源码</strike>将源添加到此目标是将它们包含在构建中的方式。</p>
        <p><b>注意:</b></p>
        <p>cmake_minimum_required()也被Zephyr的包调用。这2个版本中的最新版将会被Cmake强迫执行。
    </li>
    <li>
        <p>设置Kconfig的配置选项。参考<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/application/index.html#application-kconfig">Kconfig配置</a></p>
    </li>
    <li>
        <p>配置任意你的应用需要的覆盖设备树（devicetree overlays）。参考<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/guides/dts/howtos.html#set-devicetree-overlays">设置覆盖设备树</a></p>
        <p><b>注意：</b></p>
        <p>include($ENV{ZEPHYR_BASE}/cmake/app/boilerplate.cmake No_POLICE_SPOCE)仍然对更老的应用有着向下兼容性的支持。<strike>包括直接在例子中的boilerplate.cmake仍需去跑source zephyr-env.sh或者执行（execute）zephyr-env.cmd在构建应用前</strike>在示例中直接包含boilerplate.cmake 仍然需要在构建应用程序之前运行source zephyr-env.sh 或执行（execute）zephyr-env.cmd。</p>
    </li>
</ol>
<h2>设置变量</h2>
<h3>Option1：仅限一次</h3>
<p>设置环境变量MY_VARIABLE为foo为了你当前窗口终端的剩余时间【在当前终端窗口的生命周期内将环境变量 MY_VARIABLE 设置为 foo：】。</p>
<pre>#Linux and macOS
export MY_VARIABLE=foo
#Windows
set MY_VARIABLE=foo</pre>
<p><b>Warning：</b></p>
<p>这是最好的实验。如果你关掉你的终端窗口，使用另一个终端窗口或者选项卡（tab）、重启你的电脑等，这个设置将会永远丢失。</p>
<p>如果你想要持续使用设置，使用Options2或者3是推荐的方案。</p>
<h3>Option2：在所有terminal</h3>
<p><b>macOS和Linux：</b></p>
<p>添加export MY_VARIABLE=foo这行进位于你<strike>根</strike>主目录的shell的启动脚本。对于Bash，通常是~/.bashrc在Linux以及~/.bash_profile在macOS。改变他们的启动脚本不会影响已经启动的shell实例；尝试打开一个新的terminal window去得到新的设置。</p>
<p><b>Windows：</b></p>
<p>你可以在cmd.exe中使用setx程序或者第三方（third-party）的Rapid EE程序。</p>
<p>要使用setx，请键入此命令（type this command），然后关闭terminal window。任何新的cmd.exe窗口的MY_VARIABLE将已经被设为foo。</p>
<pre>setx MY_VARIABLE foo</pre>
<p>要install RapidEE，一个免费的（freeware）图形化的（graphical）环境变量编辑器，在一个管理员命令提示符（command prompt）下<a href="https://chocolatey.org/packages/RapidEE">使用Chocolatey</a>：</p>
<pre>choco install rapidee</pre>
<p>接着你可以从你的terminal中运行rapidee去启动程序并且设置环境变量。确保使用“User”环境变量区域——否则，你不得不以管理员身份运行RapidEE。也要确保在退出前点击在左上角的保存按钮来保存你的修改。你在RapidEE中的设置将在你打开新的terminal window时始终有效。</p>
<h3>Option3：使用zephyrrc文件</h3>
<p>如果你不希望变量的设置对所有你的terminal都有效的话，但是仍旧想去保存读入你正使用的Zephyr的环境值选择这个选项。【如果您不想让所有终端都可以使用该变量的设置，但仍想保存该值以在使用 Zephyr 时加载到您的环境中，请选择此选项。】</p>
<p><b>macOS和Linux：</b></p>
<p>创建一个名为~/.zephyrrc的文件，如果不存在的话，然后向里面添加以下行：</p>
<pre>export MY_VARIABLE=foo</pre>
<p>为了将这个值恢复到（back into）当前terminal环境中，你必须从主zephyr文件夹中运行source zephyr-env.sh。除此之外，这个脚本来源 ~/.zephyrrc。（Among other things——除其他事项外）</p>
<p>如果你关掉这个窗口这个值将会丢失，<strike>例如</strike>再次运行source zephyr-env.sh<strike>再一次得到它</strike>以将其恢复。</p>
<p><b>Windows：</b></p>
<p>用如Notepad++这种文本编辑器添加set MY_VARIABLE=foo这行进文件%userprofile%\zephyrrc.cmd中并保存该值</p>
<p>为了将这个值恢复到（back into）当前terminal环境中，当你改变目录至主zephyr目录后，你必须在一个cmd.exe窗口中运行zephyr-env.cmd。除此之外，这个脚本运行%userprofile%\zephyrrc.cmd</p>
<p>如果你关掉这个窗口这个值将会丢失，再次运行zephyr-env.cmd以将其恢复。</p>
<p>这些scripts</p>
<ul>
    <li>ZEPHYR_BASE（见下）设置zephyr的文件夹位置。</li>
    <li>添加一些特定的Zephyr位置（例如zephyr的scripts文件夹）至你的PATH环境变量。</li>
    <li><strike>从zephyrrc文件添加任意的配置，在<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/application/index.html#env-vars-zephyrrc">Options3：使用zephyrrc文件</a>中被描述。</strike>从上面<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/application/index.html#env-vars-zephyrrc">Option 3：使用 zephyrrc 文件</a>中描述的 zephyrrc 文件加载任何设置。</li>
</ul>
<p>因此当你需要<strike>他们</strike>任何的设置时你可以随时使用它们</p>
<h3>Option4：使用Zephyr构建配置CMake包</h3>
<p>如果你想要让这些你项目中的设置变量在所有用户间共享，那就选择这个设置。</p>
<p>使用<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/guides/zephyr_cmake_package.html#cmake-build-config-package">Zephyr构建配置CMake包</a>允许你在<strike>目录</strike>存储库中去<strike>确认</strike>提交共享设置，以至于所有用户可以共享它们。</p>
<p><strike>当你打开一个新的terminal时，有必要关闭正在运行的source zephyr-env.sh和zephyr-env.cmd。</strike>它还消除了在打开新终端时运行 source zephyr-env.sh 或 zephyr-env.cmd 的需要。</p>
<h2>重要的系统构建变量</h2>
<p>你可以使用很多变量去控制Zephyr构建系统。这个块描述了很多重要的Zephyr开发者需要知道的东西。</p>
<p><b>Note</b></p>
<p>BOARD,CONF_FILE和DTC_OVERLAY_FILE这些变量可以支持3种方式（按优先级的次序）（precedence——优先级）去构建这个系统。</p>
<pre>
<ul>
    <li>通过-D命令行切换west bulid或者cmake调用参数。如果你有着复数的覆盖文件
    （overlay file 实在不懂），你需要使用<strike>这个引用</strike>引号，“file1.overlay;file2.overlay”</li>
    <li>作为<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/application/index.html#env-vars">设置变量</a></li>
    <li>在你的CMakeList.txt作为set(<VARIABLE><VALUE>)指令</li>
</ul></pre>
<ul>
    <li><p><b>ZEPHYR_BASE</b>:用于构建系统的Zephyr基础变量。fing_package(Zephyr)将会自动的设置<strike>作为CMake的缓存</strike>为缓存的CMake变量。但是ZEPHYR_BASE也可以被设置为一个环境变量为了强制CMake去使用一个特殊的Zephyr装载。</p></li>
    <li><p><b>BOARD</b>:<strike>选择的构建应用所在的板将使用默认的配置</strike>选择应用程序的构建将用于默认配置的板。<strike>参考内置板的<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/boards/index.html#boards">支持的板</a>有关添加板支持的信息，<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/guides/porting/board_porting.html#board-porting-guide">板子接口指导（我*你🐎，这怎么翻译）</a>为了增加板级支持的信息</strike>有关添加板支持的信息，请参阅内置板的<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/boards/index.html#boards">支持板</a>和<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/guides/porting/board_porting.html#board-porting-guide">板移植（Porting）指南</a>。</p></li>
    <li><p><b>CONF_FILE</b>：<strike>指导</strike>表示一个或者多个Kconfig控制段文件的名字。复数的文件名可以被空格或者分号分隔。每个文件include Kconfig配置值去覆盖默认的配置值。</p>
    <p>参考<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/guides/build/kconfig/setting.html#initial-conf">The Initial</a>以获得更多的信息。</p></li>
    <li><p><b>OVERLAY_CONFIG</b>：另外的（additional）Kconfig配置段文件。复数的文件名可以被空格或者分号分隔。这将会很有用为了使CONF_FILE<strike>作</strike>保留（leave A at——将A留在）为默认值，但“混合”一些额外的配置选项。</p></li>
    <li><p><b>DTC_OVERLAY_FILE</b>：使用一个或多个设备树覆盖文件。复数的文件可以被分号或者空格分隔。参考<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/guides/dts/howtos.html#set-devicetree-overlays">set devicetree overlays</a>的例程并且参考<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/guides/dts/intro.html#devicetree-intro">Introduction to devicetree</a>获得关于Zephyr和设备树的信息</p></li>
    <li><p><b>ZEPHYR_MODULES</b>：一个CMake列表包含源码、Kconfig等位于的额外目录的绝对路径。被使用于应用的构建中。参考<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/guides/modules.html#modules">Modules(External projects)</a>获取更多细节。</p></li>
</ul>
<h2>应用CMakeList.txt</h2>
<p>每个应用必须要含有一个CMakeList.txt文件，这个文件是一个构建系统的入口点（entry point）、顶层。最终的zephyr.elf镜像包含应用和Kernel lib.</p>
<p>这节讲了在你的CMakeList.txt里你可以做什么。确保按顺序执行下面的步骤。</p>
<ol>
    <li><ul>
        <li><p>任何先前在CMake缓存中被使用的值拥有最高优先级。【由 CMake 缓存确定的任何先前使用的值具有最高优先级。】这确保了你无法在设置构建选项时去构建一个不同的BOARD值。【这可确保您不会尝试使用与构建配置步骤中设置的 BOARD 值不同的 BOARD 值运行构建。】</p></li>
        <li><p>使用-DBOARD=YOUR_BOARD在CMake命令行被设置的任何值（直接或间接通过west build）将<strike>在再一次使用时</strike>被检查和使用。</p></li>
        <li><p>如果一个<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/application/index.html#env-vars">环境变量</a>BOARD被设置，这个值将在之后被使用。【如果设置了环境变量 BOARD，则将使用其值。】</p></li>
        <li><p>最后，如果你在这个<strike>教程</strike>步骤设置了你应用的CMakeList.txt里的BOARD值，它将会被使用。</p></li>
    </ul></li>
    <li>
        <p>如果你的应用使用了一个或多个配置文件或，<strike>除了</strike>而不是平常的prj.conf（或者prj_YOUR_BOARD.conf，YOUR_BOARD是板名），<strike>添加行来设置这些适当的</strike>请适当的向这些文件添加设置的CONF_FILE变量行。如果输入复数文件名，用单个空格或者分号分割开。CMake列表可以被设置去构建配置段文件以一种模块化的方式，当你想要避免<strike>在一个独立的地方</strike>在单个位置设置CONF_FILE。例如：</p>
        <pre>set(CONF_FILE "fragment_file1.conf")
list(APPEND CONF_FILE "fragment_file2.conf")</pre>
        <p>参考<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/guides/build/kconfig/setting.html#initial-conf">The Initial Configuration</a>以获得更多信息。</p>
    </li>
    <li>
        <p>如果你的应用使用devicetree overlay，你也许需要去设置DTC_OVERLAY_FILE。参考<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/guides/dts/howtos.html#set-devicetree-overlays">Set devicetree overlays</a>。</p>
    </li>
    <li>
        <p>如果你的应用有它自己的kernel配置选项，创建一个Kconfig文件在与你应用的CMakeList.txt相同的目录下。</p>
        <p>参考<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/guides/build/index.html#kconfig">the Kconfig section of the manual（手册）</a>获取Kconfig文档的细节。</p>
        <p>一个（不太可能的）高级用例<strike>将取决于</strike>是，如果你的应用<strike>是否</strike>有独特<strike>的与构建配置不一样</strike>的配置选项，这些选项根据构建配置进行不同的设置。</p>
        <p>如果你仅想为已经存在的Zephyr配置选项设置应用专用的值，参考上面的CONF_FILE描述。</p>
        <p>像这样构造你的Kconfig文件：</p>
        <pre># SPDX-License-Identifier: Apache-2.0
mainmenu "Your Application Name"
# Your application configuration options go here
# Sources Kconfig.zephyr in the Zephyr root directory.
#
# Note: All 'source' statements work relative to the Zephyr root directory (due
# to the $srctree environment variable being set to $ZEPHYR_BASE). If you want
# to 'source' relative to the current Kconfig file instead, use 'rsource' (or a
# path relative to the Zephyr root).
source "Kconfig.zephyr"</pre>
        <p><b>Note</b></p>
        <p>source中的环境变量描述（statement——描述）被直接拓展的【source 语句中的环境变量是直接展开的】，因此你没必要去定义option env="ZEPHYR_BASE"这样的Kconfig”跳转（bounce——跳转）“符号。如果你用了这样的符号，它必须和环境变量的名字一样。</p>
        <p>参考<a href="https://developer.nordicsemi.com/nRF_Connect_SDK/doc/1.7.1/zephyr/guides/build/kconfig/extensions.html#kconfig-extensions">Kconfig extensions</a>获得更多信息。</p>
        <p>当位于应用目录中，Kconfig文件自动地被探测，但是它也可能被找到<strike>无论在哪</strike>在其他地方，如果CMake的KCONFIG_ROOT变量被设为一个绝对地址。</p>
    </li>
    <li>
        <p>在上面步骤任意一行之后，在新的一行指定需要Zephyr的应用：</p>
		<pre>find_package(Zephyr)
project(my_zephyr_app)</pre>
		<p><b>Note</b></p>
		<p>find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})可以被用于如果强制特定的Zephyr安装程序，通过清楚的（explicitly）设置了被支持的ZEPHYR_BASE环境变量。【如果应该支持通过显式设置 ZEPHYR_BASE 环境变量来强制执行特定的 Zephyr 安装，则可以使用 find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})。】所有的Zephyr例程都支持ZEPHYR_BASE环境变量。</p>
    </li>
    <li>
        <p>现在添加任意的应用程序源码文件在‘app’目标lib中，每个都在他们自己的行，比如：</p>
		<pre>target_sources(app PRIVATE src/main.c)</pre>
		<p>下面是一个简单的CMakeList.txt例子：</p>
		<pre>set(BOARD qemu_x86)
find_package(Zephyr)
project(my_zephyr_app)
target_sources(app PRIVATE src/main.c)</pre>
		<p>CMake属性（property）的HEX_FILES_TO_MERGE<strike>杠杆率（leverage）</strike>,通过Kconfig和CMake提供应用的配置，使你额外的hex文件与生成的hex文件<strike>融合</strike>合并生成Zephyr应用，例如：</p>
		<pre>set_property(GLOBAL APPEND PROPERTY HEX_FILES_TO_MERGE
    ${app_bootloader_hex}
    ${PROJECT_BINARY_DIR}/${KERNEL_HEX_NAME}
    ${app_provision_hex})</pre>
    </li>
</ol>
<h2>CMakeCache.txt</h2>
<p>CMake使用CMakeCatch.txt文件作为固有的键值对（key/value）字符串存储被用于在运行之间缓存值，包括引入编译和构建选项和从属lib【依赖库】路径。这个缓存文件当CMake在一个空文件夹中被运行时被创建。【当 CMake 在空构建文件夹中运行时，会创建此缓存文件。】</p>
<p>关注更多的CMakeCatch.txt文件细节，参考官方CMake文件<a href="http://cmake.org/runningcmake/">runningcmake</a>。</p>
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
