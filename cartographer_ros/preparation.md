# preparation

## 准备工作
1. 已经熟悉ros软件的安装，rviz显示，CMakeLists编辑。
2. 已经安装了cartographer和cartographer_ros.
3. 已经阅读了[cartographer_ros wiki](https://google-cartographer-ros.readthedocs.io/en/latest/)
4. 测试cartographer_ros部分的代码时，请使用官方原版的cartographer和编辑版的[cartographer_ros](https://github.com/wzAuto/cartographer_ros.git)。编辑版包含额外的测试代码。

## 本章目的
1. 学习使用cartographer_ros自带的工具。
2. 使用cartographer_ros实现建图功能。
3. 使用cartographer_ros实现定位功能。

## 遵循的原则
1. 尽可能使用cartographer_ros自带的工具或者基于这些工具进行改进。
2. 每一个例子都对应一个roslaunch文件。