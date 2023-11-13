#### makefile的由来
1. 项目简单时，直接利用g++的语法编译即可
    ```shell
    # g++ 默认规则编译 目标文件名为main
    g++ main.cpp factorial.cpp printhello.cpp -o main

    # 运行目标文件main
    ./main
    ```
2. 但是项目文件繁多时(成千上万),比如linux内核。如果直接g++这样编译每次把所有文件编译一遍要耗费很多时间，因为改动可能只是改动一个文件，但是编译却要把所有文件都要编译一遍。所以我们可以只对改动的文件进行编译
    ```shell
    # 只对main.cpp进行编译，生成main.o
    g++ main.cpp -c 

    # 只对factorial.cpp进行编译，生成factorial.o
    g++ factorial.cpp -c

    # 只对printhello.cpp进行编译，生成printhello.o
    g++ printhello.cpp -c
    
    # 把所有的.o文件都链接起来，生成目标文件main
    g++ *.o -o main

    # 运行目标文件main
    ./main
    ```
3. 在2的情况下是可以缩短编译时间，但是带来的问题是当文件非常多的时候，每次都手动这样输入命令非常麻烦,每次需要在命令行下输入g++需要编译的命令，完全可以写成一个脚本文件，这个脚本文件有个固定的格式，就叫makefile
    1. VERSION 1
    ```makefile
    #VERSION 1

    # hello目标依赖于main.cpp printhello.cpp factorial.cpp
    hello: main.cpp printhello.cpp factorial.cpp
    # 使用tab符后面的g++ -o hello main.cpp printhello.cpp factorial.cpp生成这个目标
        g++ -o hello main.cpp printhello.cpp factorial.cpp
    ```
    ```shell
    #在shell的命令行通过make命令对makefile文件进行编译
    make
    ```
    VERION1版本缺点：如果要编译的源文件非常多，那么每次都会把这些源文件进行编译，所以编译时间很长

    2. VERSION 2
    ```makefile
    #VERSION 2
    CXX = g++
    TARGET = hello
    OBJ = main.o printhello.o factorial.o 

    ```


    参考视频：
    【Makefile 20分钟入门，简简单单，展示如何使用Makefile管理和编译C++代码】 https://www.bilibili.com/video/BV188411L7d2/?share_source=copy_web&vd_source=a87169b88877fd501e8ba925a0512fde
#### cmake的由来
1. 帮你根据不同平台及编译环境自动生成makefile
2. 可以跨平台编译
