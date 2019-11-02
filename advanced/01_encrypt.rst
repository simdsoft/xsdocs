加密解决方案
===============

-------
特性
-------

* 高性能加密，加密命令行优先使用Intel AES NI硬件加密指令进行加密，同时还支持多线程加密
* 高性能解密，内存中AES-CBC解密是复用原始内存的，而解压缩过程会从签名中获取原始数据大小，有效避免解压过程中不必要的内存分配和释放
* 支持所有类型文件加密，对Cocos2d-X除了.ttf;.mp3;.ogg需要Streaming模式使用（一般是音频播放和字体解析内存占用，性能需要）外，其他所有文件类型均可加密
* 相对更安全，拒绝以字符串傻瓜式密钥存储方式，提高破解难度
* 默认先zlib压缩再加密，最终APK或iPA包体大小和不加密基本一样；如果不先压缩，那么加密后文件数据冗余信息极大减少，普通zip加密算法压缩后大小不会减小会导致最终包体很大，而不论是APK, 还是iPA都是zip算法压缩格式

---------
使用步骤
---------
1. 下载x-studio软件 https://x-studio.net/dl.php?host=local 并安装
#. 接下来，就可以使用命令行加密资源了

 * ``-cfg=[file]``: 指定加密配置文件，用于加密工具保存加密密钥及其他加密选项
 * ``-i=[path]``: 指定加密输入目录
 * ``-o=[path]``: 指定加密输出目录
 * ``-j2``:                  启用双线程加密资源
 * ``-ft=*.png;*.csb``:      指定资源加密文件类型

 * ``-dc=.ttf;.mp3;.ogg``:   指定直接拷贝文件类型，某些类型文件可能不需要加密

 示例命令： ``"%XS_INSTDIR%\x-studio.exe" -c -enc -cfg=D:\encrypt-cfg.xml -i=D:\OriginalRes1 -o=D:\EncryptedRes1``
 更多参数，请使用如下命令查看： ``"%XS_INSTDIR%\x-studio.exe" -c --help``

3. 注意事项
 * 目前解密运行库在Cocos2d-x-3.3及以上版本是支持的（只要未使用API: ``getFileDataFromZip``）, 但3.10及以下版本，win32需要将 **FileUtilsWin32** 构造函数的访问控制权限由 ``private`` 修改为 ``protected`` 
 * 初次加密，加密工具会自动随机生成AES-CBC加密模式所需ivec和key, 并且加密完成后会存储到encrypt-cfg.xml文件中，以便在解密运行库中设置密钥
 * 如果由-cfg选项指定的加密配置文件已存在, 那么工具从中读取加密选项, 但是如果相同选项在配置文件和命令行参数中都有指定，那么命令行参数会覆盖加密配置文件中的参数, 并更新配置文件
 * 如果指定encrypt-cfg.xml已存在, 并且需要变更密钥，那么你只需要从配置文件中删除ivec和key元素即可
 * Cocos2d-X Demo地址: https://github.com/simdsoft/x-studio/tree/master/encrypt-demo/cpp-empty-test ， 基于Cocos2d-X-3.17.1
 * 对于Lua工程，Win32平台请将加密密钥设置代码移动至:SimulatorWin.cpp文件的SimulatorWin::run()中，同时AppDelegate.cpp添加预处理器判断，如图所示:

  |figure_1|

  |figure_1|

.. |figure_1| image:: ../img/c4s1_01a.png
.. |figure_2| image:: ../img/c4s1_01b.png
