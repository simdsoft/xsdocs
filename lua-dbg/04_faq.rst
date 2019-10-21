常见问题
=================================

Q: 如何传递将设备尺寸参数传递给被调试程序?
---------------------------------------
A: 在游戏开发过程中，通常都有设计尺寸和设备尺寸以及朝向，那么，使用x-studio365启动调试时
可将这些参数传递给被调试程序，首先在.xsxproj工程文件中添加 ``debug-options`` 节点下添加 ``passing-view-args``
(10.0.5900.199及之前版本，请添加 ``pass-size-args``) 元素, 值为1，那么调试器会传递如下格式参数:
``--design-size=720x1280 --device-size=640x960 --orientation=landscape or --orientation=portrait``

.. code-block:: c++

  /* 解析C++代码参考 */
  namespace xscmdl
  {
    static inline bool check_arg(const char* arg, const char* name) { return 0 == stricmp(arg, name); }
    
    template <size_t _Size>
    static inline bool check_arg(const char* arg, const char (&name)[_Size], size_t& n)
    {
      n = _Size - 1;
      return 0 == strnicmp(arg, name, n);
    }
  }

  int main(int argc, char** argv) {
    std::string_view value;
    size_t len = 0;
    for (int i = 0; i < argc; ++i) {
      if (xscmdl::check_arg(argv[i], "--design-size=", len)) {
        value = argv[i] + len;
      }
    }
  }
 
Q: 断点混乱怎么办？
------------------
A: 我们都知道，有不少调试器在调试过程中默认是禁用编辑的，例如VS C#等，为方便代码编写，
x-studio365是允许修改代码的，多数情况下，会自动映射断点行；但少数情况会映射失败，
此时关闭文档（Ctrl+W），重新打开即可，再次启动调试，就可以解决混乱了。


Q: 断点莫名其妙无法命中怎么办？
-----------------------------
A: 检查是否是大小写问题，由于Windows是不区分文件大小写的，因此即使代码中require路径大小写
和实际磁盘不一致，依然可以成功加载脚本，但此时，不一致的代码文件断点是无法命中的。

Q: 为什么在文档中查看代码已经运行，但却没有实际输出？
-----------------------------
A: 开发目录和exe所在目录是否都有相同文件名代码，cocos引擎默认会优先加载exe所在目录下的代码，
开发过程中，始终保持工程根目录下的src目录一份代码，并删除exe所在目录下的代码是一个良好的开发习惯，这是
早起cocos lua的设计缺陷，应该摒弃。
