# 一个游戏开发的个人Wiki

---

（更新缓慢，大部分时间只在本地，偶尔 push 到 Github）

# 一、C++

## C++ 辅助工具

- [https://godbolt.org/](https://godbolt.org/)，在线c++编译运行网站，无需登陆注册无广告纯免费；可选择各种类型的c++编译器以及各个版本，快速验证STL的API用法时很有用；可查看对应代码的汇编，优化某小段代码时很有用；可于他人快速分享，分享链接长期有效，提问或回答问题举例子时很有用；类似的还有编译器视角工具[https://cppinsights.io/](https://cppinsights.io/)，性能对比工具 [https://quick-bench.com/](https://quick-bench.com/)
- [VLD](https://kinddragon.github.io/vld/)，Visual Studio Detector, 可集成进Visual Studio的内存泄漏检测工具，用法简单，一个静态库+一个头文件即可，不需要写额外代码；很久没更新了，看样子被废弃了。另一个2019版本[[Github](https://github.com/oneiric/vld)]

## C++ 语法解读

- [https://en.cppreference.com/w/](https://en.cppreference.com/w/ )， C++标准查阅手册，也有[[中文版](https://zh.cppreference.com/w/%E9%A6%96%E9%A1%B5)]的，但标准规范字字珠玑，英文版更准确； 网站右上角可以点击Edit，编辑得到本地副本，下次再打开该网页，看到的就是个性化的本地副本；也可把自己的副本提交审查，审查通过就算是贡献者啦。官方网站有点丑，也可以看这个规范汇总网站[[Runebook](https://runebook.dev/en/docs/cpp/)]，支持快捷搜索
- [Thriving in a Crowded and Changing World: C++ 2006–2020](https://dl.acm.org/doi/pdf/10.1145/3386320) ， 写于2020，作者是C++创始人兼标准委员会，讲述了C++标准发展到2020年的过程中，各个标准的重大更新，以及设计考量，keyword的设计渊源，另外重点介绍了C++17/20的一些新标准，还可以了解到C++标准委员会这个组织的运行机制和C++23标准的发展趋势
-  [https://hackingcpp.com/](https://hackingcpp.com/)，国外个人开发者维护的C++学习网站；对C++生态介绍的很全面（STL入门，各大编译器介绍，各家构建系统，包管理器介绍，各家IDE介绍，各种单元测试(UT)框架介绍；非常非常适合纯小白入门C++；适合也开发人员查缺补漏；  2023.05工具系统一栏细读+1
- [高速上手C++11/14/17/20](https://changkun.de/modern-cpp/zh-cn/00-preface/) ， 写于2020的电子书， 作者是CS phD, 开发者个人经验总结；快速过一遍新语法的使用方法
- [C++ Rvalue References Explained](http://thbecker.net/articles/rvalue_references/section_01.html) ， 某国外程序员写的解读系列；由浅入深，从需求出发，介绍为什么会有右值这个概念，逐步引出左右值，移动语义，完美转发等概念 
- [Memory Barriers Are Like Source Control Operations](https://preshing.com/20120710/memory-barriers-are-like-source-control-operations/) ， 2013年的文章；某国外游戏程序写于2012年；介绍了memory order背后的memory barrier概念，从而更好理解std::atomic ， 2021细读+1；2023.06通读+1；

## C++ 学习社区

- [cppweekly](https://www.youtube.com/@cppweekly) ，Youtube上的一个博主频道，C++语法专家；似乎是全职博主，更新频率非常高；主要讲c++各种语法上的技巧（例如`constexpr`的所有应用场景），STL接口使用的注意事项；
- [Vulkan Community](https://community.khronos.org/c/vulkan/24) ， Vulkan官方提问社区，以提问问题-别人回答为主；非常活跃，会有经验丰富的大佬回复，而且回复速度快（前提是一个讲清楚且不傻瓜的问题）

## C++ 编码风格

- [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines) ， 官方的C++指导手册，提供一些实践建议：“xxx，你最好这样用”   ；参考性中等，不是所有建议都值得采纳，选出几个条加入自己的项目就够了 ，例如 Prefer enumerations over macros；Prefer `class enum MyEnum {}; `over  `enum MyEnum {};` 
- [OpenSSF Best Practices Badge](https://github.com/coreinfrastructure/best-practices-badge) ， 一个开源项目，用于评估其他开源项目是否符合FLOSS(Free/Libre and Open Source Software)标准。 但条件略苛刻，个人项目完全没必要通过FLOSS标准。但如果你想知道一个正经的开源项目，应该具备哪些要素，可通过查阅FLOSS标准了解到。FLOSS标准的部分要求：一个正经的开源项目，应当有一个稳定的主页，应当有How-to-install文档说明等 
- [Clang-Format Style Options](https://clang.llvm.org/docs/ClangFormatStyleOptions.html) ， clang-format的官方文档，通过查阅此文档，编写自己的`.clang-format`文件，即可自定义项目的代码风格； 如果不想从零编写自己的`.clang-format`文件，可[basedonstyle](https://clang.llvm.org/docs/ClangFormatStyleOptions.html#basedonstyle)，或者把已有风格导出为文件，再二次编辑；如果想某些文件夹下的文件被formatted，某些不会，参考选项`DisableFormat: true`；如果想文件内某几行不被format，查看用法[Disabling Formatting on a Piece of Code](https://clang.llvm.org/docs/ClangFormatStyleOptions.html#disabling-formatting-on-a-piece-of-code)；如果项目中途引入或更换代码的格式化风格，但又怕污染git history，查看用法[` .git-blame-ignore-revs `](https://docs.github.com/en/repositories/working-with-files/using-files/viewing-a-file#ignore-commits-in-the-blame-view) 
-  [Clang-Tidy Check Lists](https://clang.llvm.org/extra/clang-tidy/checks/list.html) ， 官方clang-tidy的检查表文档，通过查阅此文档，获取可支持的所有checks ， 如果觉得checks太多，了解不过来，建议拷贝知名Github开源项目的`.clang-tidy`文件，在其基础上二次编辑；如果你想手动地为某些特例关掉检查：[Suppressing Undesired Diagnostics](https://clang.llvm.org/extra/clang-tidy/#suppressing-undesired-diagnostics)；例如readability-identifier-naming中可以加入检查，要求所有的宏定义，必须是`UPPER_CASE`风格
- [Leak-Freedom in C++... By Default](https://www.youtube.com/watch?v=JfmTagWcqoE) ， CppCon2016的一个talk，介绍了一种指针的规范使用风格，如什么时候用裸指针，什么时候用什么类型的智能指针，来避免内存泄漏问题 ， 但老实说，也避免不了内存泄漏（没GC就永远有泄漏的风险？）；不过，规范的使用，定死每种指针的使用场景范围，能极大增加代码的可读性 ， 一张图快速概览：[Leak_Freedom_Strategy](https://raw.githubusercontent.com/NoSW/CloudImg/PicGo/img/202308040041415.png) 
- [epic-cpp-coding-standard-for-unreal-engine](https://docs.unrealengine.com/5.2/en-US/epic-cplusplus-coding-standard-for-unreal-engine/) ， Unreal的官方文档，介绍了Unreal中C++的编写标准；如果想为自己的项目指定一个C++编码标准，这会是很好的一个参考对象。 ， 例如，规定内置类型必须使用`int16`, `uint32`等，而永远不应该使用`short`，`unsigned int` 等
- [Procedures and Conventions in Vulkan](https://registry.khronos.org/vulkan/specs/1.3/styleguide.html) ， Vulkan的官方文档，介绍了Vulkan中结构体，API，宏定义的命名约定，规范 ， 如何要设计一套不断迭代，长期更新维护的API，这会是很好的一个参考对象。 ， 例如，要求API的接口必须以`vk`为前缀，同时后面紧接的必须是一个(及物)动词；规定了返回值只能为错误码或者void；错误码不是全局的那种latest error设计
- [Back to Basics: C++ API Design](https://www.youtube.com/watch?v=zL-vn_pGGgY) ， CppCon2022 一个talk，介绍了API设计时的几个原则和建议，包括如何API命名、参数命名、属性修饰等 ， 废话太多；建议直接拖到最后15分钟；一张图快速概览：[Back to Basics: C++ API Design](https://raw.githubusercontent.com/NoSW/CloudImg/PicGo/img/202307222308883.png)


## C++ 第三方库 （通用）

- [cameron314/readerwriterqueue](https://github.com/cameron314/readerwriterqueue)， 单消费者-单生产者模型的无锁队列实现；head-only；无三方依赖，非常容易继承；有单测，有benchmark； ， 作者本人写过几篇实现原理的解释文章，可用于学习无锁数据结构；API简洁；侧重性能，可显式地通过API来控制，当空间不够时，是否自动地动态分配内存；
- [cameron314/concurrentqueue](https://github.com/cameron314/concurrentqueue)， 多消费者-多生产者模型的无锁队列实现；head-only；无三方依赖，非常容易继承；有单测，有和TBB；Boost对比的benchmark
- [zpp_bits](https://github.com/eyalz800/zpp_bits),  **a  lightweight C++20 serialization and RPC library**,始于2021.12，作者背景不知道。是 一个性能似乎是目前最好的C++二进制序列化库，用C++20写的，不兼容20以前的库，有很多 compile-time computation的技巧可以学习，也可以拿来学习C++20的一些特性实践，作者还把自己的库和各大同类流行库做了个benchmark，放在了README，可以了解到各大序列化库的使用方法。
- [nativefiledialog](https://github.com/mlabbe/nativefiledialog)， a tiny, neat C library that portably invokes native file open, folder select and save dialogs. 始于2017.11，作者是Frogtoss Games公司的。多个OS的FileDialog实现，但是内部直接用的malloc/free效率不行，需要重写。 ， 虽然已经不怎么更新了，但还能用；另外有个[fork](https://github.com/aarcangeli/nativefiledialog-cmake)为其添加了CMake配置，方便很多
- [mimalloc](https://github.com/microsoft/mimalloc)，a general purpose allocator with excellent performance characteristics. 微软官方出品，还有配套论文，多平台维护ing。malloc/free的替代品。README也有和各大同类流行库个benchmark对比。源码里很多内存分配的技术概念可以学习，不想看也可以clone下来直接用。 原理讲解论文：[[pdf](https://www.microsoft.com/en-us/research/uploads/prod/2019/06/mimalloc-tr-v1.pdf)]，部分设计已经在源码中被替换了

## CPU&GPU

- [上帝视角看GPU](https://www.bilibili.com/video/BV1P44y1V7bu/?share_source=copy_web&vd_source=bbd1311a6797aa72ff9fcb948b40db33) ， 2022.04  ， 个人，资深图形开发，B站视频教程 ， 总时长约一小时，纯干货；从图像处理的角度介绍GPU架构的发展演化历程；介绍图形流水线各个部件如何映射到GPU硬件上以及发展趋势；从GPU到API的软件栈；光栅化的实现原理；硬件光线追踪的发展趋势；需要有渲染流水线的基础前置知识，即IA+VS+TES+TCS+Raster+FS+OM大致流程； 差最后一节光追未看；
-  [Latency Numbers Every Programmer Should Know](https://gist.github.com/jboner/2841832) ； Lightbend的技术创始人，CPU访存操作的延迟时间估计，10年前的CPU性能数据了，参考性一般；但也能看得出主存访问相比L1高出10\^2\~10\^3量级，相比L2高出10\~10^2量级。原文：http://norvig.com/21-days.html#answers
- [How GPU Computing Works](https://www.nvidia.com/en-us/on-demand/session/gtcspring21-s31151/) ， 2021.04，作者是NVIDIA的CUDA架构师 ，讲解GPU的工作机制，不是介绍CUDA本身；[B站英文字幕版](https://www.bilibili.com/video/BV1gD4y1i7NN/?vd_source=df29b8390bef461968dfdef3a58f945d)，[知乎中文字幕版]( https://www.zhihu.com/question/49313492/answer/2232175115)；观点是memory latency>>flops，CPU侧重使用不同的memory(register, L1, L2,...)减小latency，GPU侧重基于大量数据使用不同memory通过大量线程并行处理隐藏latency;GPU设计的工作量远超它的工作时间；吞吐量和延迟，火车和汽车；所以GPU需要保持忙率，CPU需要一个线程尽快执行完任务，为下个任务让路，防止堵车；任务尽可能独立，同时允许一定数量的线程交互；带宽、延迟和flops的平衡，compute intensity = flops/Data Rate
- [[3P Perf (2019)](https://developer.nvidia.com/blog/the-peak-performance-analysis-method-for-optimizing-any-gpu-workload/)] [[3P Perf 的续篇 (2020)](https://developer.nvidia.com/blog/optimizing-vk-vkr-and-dx12-dxr-applications-using-nsight-graphics-gpu-trace-advanced-mode-metrics/)]，NVIDA 16+年经验的GameWorks技术开发组成员，使用 Nsight Graphics 的通用（3D图形，CUDA等均可套用）GPU性能分析的 High-Level 指导路径
- [Software optimization resources](https://www.agner.org/optimize/) ，丹麦技术大学一个专注于提升软件性能的研究员博客，写的5篇（~200页每篇）性能优化系列，其中只有篇1面向SDE，其他篇过于底层了。学习64bit和32bit系统的异同点，各类高级编程语言（interpreted, intermediate  code and just-in-time compiled, fully compiled）的异同优缺点以及技术选型指导，各类c++编译器的优缺点

# 二、图形API

## Vulkan开发

- [vulkan-diagrams](https://github.com/David-DiGioia/vulkan-diagrams)， 写于2020.10， 作者是AMD图程，将vulkan的基本SDK用法总结为图表；适合新手；中手，可以快速捋清Vulkan众多struct之间的关联以及流程 
- [Vulkan Configurator](https://vulkan.lunarg.com/doc/view/1.3.250.1/linux/vkconfig.html)， Vulkan开发必备；开启Validation Layer可以及时提醒API的使用错误，使用建议等问题；在Windows 上使用该图形化界面工具可以很方便地决定开启或关闭Validation Layer，而无需重新编译代码；可强制覆写代码中的配置                                                             ，      ，
- [vulkan-layer-guide](https://renderdoc.org/vulkan-layer-guide.html)， RenderDoc开发者（经验肯定非常丰富）2016.11 写的对Vulkan layer机制的简明介绍。作为Vulkan哲学之一，Layer机制非常值得学习，如何剥离API内部的runtime校验逻辑（minimal error checking），和开发期校验逻辑(sufficient error checking，可手动开启/关闭，以避免对API的错误or不规范使用)，如何方便地注入add-in，对API的鲁棒性很重要。可以考虑把这套驱动层的设计思想迁移到通用SDK开发中 ， layer system算是VK API特色了，“打桩”机制官方版；其他API要想打桩或hook得重新build动态库了，例如Android源码中的[opengl/libs](https://android.googlesource.com/platform/frameworks/native/+/refs/heads/main/opengl/libs?autodive=0%2F%2F%2F%2F/)
-  [Writing an efficient Vulkan renderer](https://zeux.io/2020/02/27/writing-an-efficient-vulkan-renderer/) ， 2020.02, 8年Roblox程序, volk库的作者，属于GPU Zen 2的其中一个章节。讲通用的VK性能指导，通读过一遍 ，                                                            
- [Synchronization-Example](https://github.com/KhronosGroup/Vulkan-Docs/wiki/Synchronization-Examples) ， 官方给的sync使用示例                                         
- [Inter-Process Texture Sharing with DMA-BUF](https://blaztinn.gitlab.io/post/dmabuf-texture-sharing/);  [VK_EXT_external_memory_dma_buf](https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VK_EXT_external_memory_dma_buf.html) ， DMA(Direct Memory Access) Buffer，一种Linux上进程间共享显存的机制；可用于实现多个相同游戏实例之间共享read-only 纹理，模型等数据 ，降低显存占用，减少数据上传的延迟（只需要一个实例上传一份），但引入了本机进程间通信延迟。作者2020年这篇博客基于OpenGL+EGL给出了一个非常完整，可运行的，最小的技术demo，实现了Linux上不同进程之间共享相同的纹理数据，从而降低总的显存占用 ， 考虑用Vulkan替换GL实现对多个GPU显存更灵活的控制；相关拓展提供了`vkGetMemoryFdKHR`等接口



## GLSL

- [The std430 Layout Rules](https://www.oreilly.com/library/view/opengl-programming-guide/9780132748445/app09lev1sec3.html)
- [The GL_ARB_enhanced_layouts](https://registry.khronos.org/OpenGL/extensions/ARB/ARB_enhanced_layouts.tx)
- [The GL_EXT_scalar_block_layout](https://github.com/KhronosGroup/GLSL/blob/master/extensions/ext/GL_EXT_scalar_block_layout.txt)

## 渲染管线

- [Forward vs Deferred vs Forward+ Rendering with DirectX 11](https://www.3dgep.com/forward-plus/) ， 2015年 ， 某大学的master毕业设计 ， 做了完整的实验，通过实验数据详尽的对比三种渲染管线，非得值得阅读 ， 结论是Forward:multi shading model+semi-transparent,or few dynamic lights；deferred：forward不合适的场景；Foward+：因为做光源剔除需要额外的内存；Clusterred Deferred：ditto

- [Deferred Rendering for Current and Future Rendering Pipelines](https://www.intel.com/content/dam/develop/external/us/en/documents/lauritzen-deferred-shading-siggraph-2010-181241.pdf)，SIGGRAPH 2010，探讨了forward，deferred，tile-baed deferred

- [Clustered Deferred and Forward Shading](https://www.cse.chalmers.se/~uffe/clustered_shading_preprint.pdf)，HPG2012

- [Future Directions for Compute-for-Graphics](https://www.ea.com/seed/news/seed-siggraph2017-compute-for-graphics)，SIGGRAPH2017

- [《原神》主机版渲染技术要点和解决方案](https://developer.unity.cn/projects/5fbb4407edbc2a0c41d52e5e)，技术总监2020年对主机版方案的分享，实用的光影技术选型方案

- [The Visibility Buffer: A Cache-Friendly Approach to Deferred Shading](https://jcgt.org/published/0002/02/04/paper.pdf)，2013年Intel发表的论文，最早剔除VisBuffer思路

- [A Rendering Architecture for high-resolution Displays and Console Games](http://diaryofagraphicsprogrammer.blogspot.com/2018/03/triangle-visibility-buffer.html)，2018年，跨平台渲染库The-Forge实现Visibility-Buffer方案，并提供了Example Code，与传统G-Buffer对比

- [Visibility Buffer Rendering with Material Graphs](http://filmicworlds.com/blog/visibility-buffer-rendering-with-material-graphs/)，2021，[FILMICWORLDS](http://www.twitter.com/@filmicworlds)发表的技术文章，通过实验对比了Forward，Deferred (G-Buffer)，Visibility-Buffer三个方案的各自适用场景

- [A Macro View of Nanite](https://www.elopezr.com/a-macro-view-of-nanite/)，2021.05，对UE4 Nanite的粗略分析

- [A Deep Dive into Nanite Virtualized Geometry](https://advances.realtimerendering.com/s2021/Karis_Nanite_SIGGRAPH_Advances_2021_final.pdf) [[Youtube](https://www.youtube.com/watch?v=eviSykqSUUw)]，官方解读， SIGGRAPH Courses, 2021

- [NANITE FOR EDUCATORS AND STUDENTS](https://cdn2.unrealengine.com/nanite-for-educators-and-students-2-b01ced77f058.pdf)，使用指导手册

- [Geometry Processing: The Nanite System in Unreal Engine 5](https://www.medien.ifi.lmu.de/lehre/ws2122/gp/slides/gp-ws2122-extra-nanite.pdf)，其他解读版本

  

# 三、图形算法

## 数学

- Quaternions Revisited，2014， GPU Pro 5
- [四元数与三维旋转](https://krasjet.github.io/quaternion/) ，写于2018 ， 个人发布的电子书 

## PBR

- [filement pbr](https://google.github.io/filament/Filament.html#materialsystem/standardmodel) ， 写于2018.8 ， google开源项目的技术文档， 可以学到材质的基础的概念和PBR的基础算法
- [THE PBR GUIDE](https://substance3d.adobe.com/tutorials/courses/the-pbr-guide-part-1)， 写于2018， substance给美术人员的技术指导手册 ，分为两个Part，理解PBR（√），创建PBR纹理（x）

## Area light

- [Area Lights](https://alextardif.com/arealights.html)，写于2016.04，作者是图形程序，介绍了Sphere， Tube，Rectangular 三种Shape的区域光实现；全是Representative Point的方法，只不过在于如何对这三种规则几何图形求closest point between ray and sphere ， 贴了shader代码；重点参考closest point的几何计算如何实现
- [Area Lights in Wicked Engine](https://wickedengine.net/2017/09/07/area-lights/)，2017年的技术博客，介绍Wicked Engine中的Representative Point方法
- [Geometric Derivation of the Irradiance of Polygonal Lights](https://hal.science/hal-01458129/document)，Unity技术人员于2017.02发布的一篇小论文，详细推导了如何计算Irradiance of Polygonal Lights（也就是半球面上三个点组成的区域的irradiance）；还配了图例，清晰易懂
- [Real Shading in Unreal Engine 4](https://cdn2.unrealengine.com/Resources/files/2013SiggraphPresentationsNotes-26915738.pdf)，UE Nanite作者 Brian Karis于2013年的文章，重点介绍PBR引入UE4的实践，顺带提到了基于Representative Point方法的Sphere，Tube这两种shape的实现；有公式方便抄；
- [Physically Based Area Lights](https://gitea.yiem.net/QianMo/Real-Time-Rendering-4th-Bibliography-Collection/raw/branch/main/Chapter%201-24/[0380]%20[GPU%20Pro5%202014]%20Physically%20Based%20Area%20Lights.pdf)，Killzone: Shadow Fall游戏团队的Michal Drobo于2014年发表在GPU Pro5上的专题论文，介绍PlayStation4上的方案；从渲染方程的推导开始介绍，文字量相对较多；不同于Representative Point；该文计算diffuse lighting时选取most important point in terms of importance sampling. 选取方法是，1）找到n于light plane的交点p' 2）找到p在light plane上的投影点p’‘；3)求 h= normalized(pp' + pp‘’)，4）h与light plane的交点即为most important point
- [Real-Time Polygonal-Light Shading with Linearly Transformed Cosines](https://eheitzresearch.wordpress.com/415-2/)；[[slides](https://drive.google.com/file/d/0BzvWIdpUpRx_Z2pZWWFtam5xTFE/view?resourcekey=0-K9rJBtyrgGtxfP3XHDUCyQ)] [[Github](https://github.com/selfshadow/ltc_code)] ， 2016     ， ACM SIGGRAPH 2016;Unity技术                           ， 学术论文     ， 事实上的标准方法，支持任意形状的多边形，例如五角星，支持texture lighting；在UE5中也被采纳 [[RectLight.ush](https://github.com/EpicGames/UnrealEngine/blob/463443057fb97f1af0d2951705324ce8818d2a55/Engine/Shaders/Private/RectLight.ush)] ， slides特意做了动画演示数学原理
- [Area Light Sources in Cyberpunk 2077](https://history.siggraph.org/wp-content/uploads/2022/06/2021-Talks-Sikachev_Area-Light-Sources-in-Cyberpunk-2077.pdf)，2022年发表的简短小论文，依然基于Representative Point的方法，但通过weighted representative direction based on roughness 更好地达到 energy preservation

## TAA

- [A Survey of Temporal Antialiasing Techniques TAA](https://www.kdocs.cn/l/ctD3yb2oykOb)，NV研究员于2020年写的一篇综述论文， 看完了一遍，捋了大致流程，优化细节还需反复阅读
- [Temporal Anti Aliasing – Step by Step](https://ziyadbarakat.wordpress.com/2020/07/28/temporal-anti-aliasing-step-by-step/)，2020年的技术博客，介绍了TAA的一般实现流程
- [High Quality Temporal Supersampling](http://advances.realtimerendering.com/s2014/epic/TemporalAA.pptx)，SiGGRAPH2014上UE4介绍了自家引擎内TAA方案的实现
- [Temporal Reprojection Anti-Aliasing in INSIDE](https://www.gdcvault.com/play/1022970/Temporal-Reprojection-Anti-Aliasing-in)，GDC2016

# 四、软件设计

## 引擎架构

- [游戏编程模式-观察者模式](https://gpp.tkchu.me/observer.html)，EA 14年+资深游戏程序，约2013写的电子书， 中译版，翻译水平很高，甚至忠实地保留的原文的排版风格；从游戏开始视角出发，实用性高；观察者模式的应用场景，不适合场景都提到了， 此外该系列还有其他常见的十几种设计模式，游戏开发向必读本

- [how-to-write-your-own-cpp-game-engine](https://preshing.com/20171218/how-to-write-your-own-cpp-game-engine/)， 写于2017年 ， 作者是（从17年来看）14年经验的资深TA，待过好几个3A工作室 ， 博客     ， 讲了代码规范；指针使用规范；序列化等议题；启发有余，干货不大；开发一款引擎，更适合作为编程技术的学习方式； ， 作者的感悟：

  >  I spent 14 years in the game industry and I’m still figuring it out. I wasn’t even sure I could write an engine from scratch, since it’s vastly different from the daily responsibilities of a programming job at a big studio. ，

- [CppCon 2016: Jason Jurecka “Game engine using STD C++ 11"](https://www.youtube.com/watch?v=8AjRD6mU96s) ，12年+经验的某3A游戏工作室资深程序，介绍了为什么少用STL；如何最大化并行，如何资源编译期的版本控制等等
  
- 虚幻引擎程序设计浅析[cloud]，2017年出版的纸质书，少有的讲源码的，讲了虚幻各个模块的代码设计，**进度在第4章**  
  
- [SMASH: a Distributed Game Engine Architecture](https://www.math.unipd.it/~cpalazzi/papers/Palazzi-engine-iscc16.pdf) ，2016
  
- [A System for Interactive Indirect Illumination in the Cloud](https://research.nvidia.com/publication/2013-07_cloudlight-system-amortizing-indirect-lighting-real-time-rendering-technical)，NVIDIA 于2013的一个talk，思路是把indirect lighting 放在云上计算                    

# 五、其他

## 排版工具

- [Markdeep](https://casual-effects.com/markdeep/)，给markdown文件头或尾加入一行html代码，使用浏览器渲染修改后的文件时，即可获得这种效果：[一个采用的markdeep的技术文档](https://google.github.io/filament/Filament.htmll)，[各种排版效果展示](https://casual-effects.com/markdeep/features.md.html)， 对懒得折腾各种排版软件、排版环境的人友好，只用写md，写完插入一行代码就可以获得还不错的排版效果
- [Overleaf](https://www.overleaf.com/latex/templates)， 在线Latex模板网站，免费无广告，免去本地latex的环境搭建之苦；登录账号，还有云存储的功能；修改、储存简历好用                                       
- [Typst](https://typst.app/)，[[github](https://github.com/typst/typst)] ， 23年刚推出的，用Rust写的开源排版工具；编程的方式写文档，比Latex语法简单，但排版能力号称不输Latex；重点是可多人实时协同编辑；目标客户看起来像是经常需要写论文科研人员 ， 目前模板库不多，只有五六个，且都是学术论文模板，其他选择较少

