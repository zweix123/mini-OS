> 参考书籍《操作系统真象还原》郑刚

# 第一章 部署工作环境

+ 项目环境为`Ubuntu20.04`（我的机器是Windows10，使用VMware workstation Pro虚拟机）

1. VMware workstation Pro的安装和使用请STFW
2. Ubuntu20.04的安装和初步配置请STFW

+ 运行项目的虚拟机是`bochs 2.6.2`（和书中使用的版本一样）

1. 下载bochs

   1. 下载软件包：

      ```bash
      wget https://udomain.dl.sourceforge.net/project/bochs/bochs/2.6.2/bochs-2.6.2.tar.gz
      ```

      在当前路径会出现一个`bochs-2.6.2.tar.gz`的文件

   2. 解压软件压缩包

      ```bash
      tar zxvf bochs-2.6.2.tar.gz
      ```

      在当前路径会出现一个`bochs-2.6.2/`目录

   3. 安装：`cd bochs-2.6.2/`

      + 安装需要的软件：

        ```bash
        sudo apt-get install -y build-essential  # build-essential packages, include binary utilities, gcc, make, and so on 
        ```

        > 如果安装失败可能是软件源的问题，请更换软件源，或者更换软件源了但是没更新

      1. configure：

         ```bash
         ./configure \ 
             --prefix=~/bochs \       # 安装路径
             --enable-debugger \      # 打开调试器
             --enable-disasm \        # 支持反汇编
             --enable-iodebug \       # 启用io接口调试器
             --enable-x86-debugger \  # 支持x86调试器
             --with-x \               # 使用x windows
             --with-x11               # 使用x11图形用户接口
         ```

         > 关于安装路径，我最开始设置在标准的`\usr\local\bochs`下，但是那样每次都要过去sudo，不方便

      2. make：

         1. 打开`Makefile`：92行末尾添加`-lpthread`

         2. 安装需要的软件

            ```bash
            sudo apt-get install -y libgtk2.0-dev  # 安装gtk运行环境
            sudo apt-get install -y gnome-devel    # 安装gtk开发环境
            ```

         ```bash
         make  # 可能需要sudo
         ```

      3. make install：

         ```bash
         make install  # 可能需要sudo
         ```

      > 如果这个过程哪里出错请重头来过，`make clean`似乎无效

+ bochs的初步配置：进入软件目录：`cd ~/bochs`（对应上面的下载位置）

  + 配置文件`bochs\bochsrc.disk`（下面的中文不要出现在文件中）

    ```bash
    megs: 32  # 内存大小，单位MB
    
    romimage: file=~/bochs/share/bochs/BIOS-bochs-latest  # 设置机器的BIOS
    vgaromimage: file=~/bochs/share/bochs/VGABIOS-lgpl-latest  # 设置VGA BIOS
    
    boot: disk  # 从硬盘启动（从软盘启动的相关配置没有在这里）
    
    log: bochs.out  # 日志文件的输出
    
    mouse: enabled=0  # 关闭鼠标
    keyboard_mapping: enabled=1, map=~/bochs/share/bochs/keymaps/x11-pc-us.map  # 打开键盘
    
    ata0: enabled=1, ioaddr1=0x1f0, ioaddr2=0x3f0, irq=14  # 硬盘设置
    
    # 远程gdb设置未配置
    ```

  + 运行：

    ```bash
    ~/bochs/bin/bochs
    [enther]
    bochsrc.disk
    [enther]
    c  # 运行在上一步时就会弹出一个新窗口，这个字符c的键入是在原本的shell，没有这个c新创建是黑屏，书上并没有说
    ```

  > 报错

  + 创建启动盘

    ```bash
    ~/bochs/bin/bximage
    hd
    flat
    60
    hd60M.img
    ```

    随后会提示将一行配置添加到上面的`bochsrc.disk`末尾

    再次启动

    > 指定配置文件启动：`~/bochs/bin/bochs -f bochsrc.disk`

    仍然报错

# 第二章 编写MBR主引导记录







+ 初步配置bochs：

  ```bash
  megs: 32
  
  romimage: file=/usr/local/bochs/share/bochs/BIOS-bochs-latest
  vgaromimage: file=/usr/local/bochs/share/bochs/VGABIOS-lgpl-latest
  
  boot: disk
  
  log: bochs.out
  
  mouse: enabled=0
  keyboard_mapping: enabled=1, map=/usr/local/bochs/share/bochs/keymaps/x11-pc-us.map
  
  ata0: enabled=1,ioaddr1=0x1f0,ioaddr2=0x3f0,irq=14
  ```

  将上面内容复制到`\usr\local\bochs\bochsrc.disk`中




