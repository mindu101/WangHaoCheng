### 核心结构
#### 文件组成：
README.md
└── seeker
    ├── CMakeLists.txt
    ├── config
    │   └── con.yaml
    ├── include
    │   └── seeker
    ├── launch
    │   └── kkk.launch.py
    ├── package.xml
    └── src
        ├── publisher.cpp
        └── subscriber.cpp

#### 组成各个模块思路：
1. seeker是包名，我是直接通过ros2 create创建的，顺带一键生成了publisher和Cmakelist以及package.xml
2. pub用话题发布每500ms一次的摄像头摄影结果，声明了gain和exposure_time 两个参数,用cv bridge来处理mat和Image类型转换，用timer定时发送；
3. sub是订阅pub，订阅成功后触发回调函数，临时保存一下发来的图片，后来用空服务来作为触发器，来永久保存图片，保存图片前用std里一个创建文件夹的函数创建
4. sub设置了一个计时器，每500ms进行一次cv里各种函数的调用，然而因为诡异的段错误一直没跑通，注释后其他模块可以正常进行。
5. sub里还进行了cv各个参数的声明，还有get
6. CMakeLists文件：
除了ros2 create一键生成的之外，还添加了sensor_msgs，std_srvs两个依赖的查找
添加一个新的可执行文件
添加ament_target_dependencies里一些项目
添加了
install(DIRECTORY
    launch
    DESTINATION share/${PROJECT_NAME}/
)
install(DIRECTORY config
  DESTINATION share/${PROJECT_NAME}/)
来便于载入参数；
7. package.xml;
除了ros2 create一键生成的之外，还添加了
<depend>std_msgs</depend>
<depend>sensor_msgs</depend>
<depend>cv_bridge</depend>
<depend>std_srvs</depend>等依赖的声明

8. kkk.launch,py
手写了一个generate_launch_description（）函数，用join来把config的相对路径来给搞成绝对路径(缝合在一起)parameters
然后用node同时启动两个节点
9. con.yaml
严格按照格式，把参数的值确定好；
10. 配置了很长时间vscode，去搞出来代码补全




