# 对应源码与构建

发布驱动对应的修改源码、补丁脚本及构建说明均在 `mcstudio-crossover-source.zip` 和单文件脚本中。导出或解压后请阅读 `source/BUILD.md`。

上游完整源码：

https://media.codeweavers.com/pub/crossover/source/crossover-sources-26.3.0.tar.gz

本地构建使用其中的 Wine 源码，应用 `source/patch-driver.py` 与 `source/cocoa_shared_gl.m` 后编译 `dlls/winemac.drv/winemac.so`。二进制采用临时签名；源码可以自行修改、构建和重新签名。LGPL 许可证见 LICENSE。
