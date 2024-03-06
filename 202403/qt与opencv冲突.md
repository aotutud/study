qt.qpa.plugin: Could not load the Qt platform plugin "xcb" in "/usr/local/lib/python3.10/dist-packages/cv2/qt/plugins" even though it was found.
This application failed to start because no Qt platform plugin could be initialized. Reinstalling the application may fix this problem.

Available platform plugins are: xcb, eglfs, linuxfb, minimal, minimalegl, offscreen, vnc.

问题在于编译 opencv 的 Qt 版本与 PyQt5 使用的版本不同，从而导致冲突。

一个简单的解决方案是删除环境中的所有 openCV

然后只需安装

pip install opencv-python-headless



error: (-2:Unspecified error) The function is not implemented. Rebuild the library with Windows, GTK+ 2.x or Cocoa support. If you are on Ubuntu or Debian, install libgtk2.0-dev and pkg-config, then re-run cmake or configure script in function 'cvNamedWindow


解决办法：安装opencv-contrib-python，如下，在终端输入命令进行安装：

pip install opencv-contrib-python
