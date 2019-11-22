在 Windows 上搭建 Flutter 开发环境

获取 Flutter SDK
flutter 官网下载 https://flutter.dev/docs/development/tools/sdk/releases#windows
Flutter github 项目 https://github.com/flutter/flutter/releases
将安装包 zip 解压到你想安装 Flutter SDK 的路径
在 Flutter 安装目录的 flutter 文件下找到 flutter_console.bat，双击运行并启动 flutter 命令行，接下来，你就可以在 Flutter 命令行运行 flutter 命令了。

export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn

由于一些 flutter 命令需要联网获取数据，如果您是在国内访问，由于众所周知的原因，直接访问很可能不会成功。 上面的 PUB_HOSTED_URL 和 FLUTTER_STORAGE_BASE_URL 是 google 为国内开发者搭建的临时镜像。
