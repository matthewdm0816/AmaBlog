---
title: "在Windows上使用`fish`: A Tutorial For Beginners"
author: "KazamiMichiru"
date: "2021-3-23"
updatedAt: "2021-3-23"
---

## 在Windows上使用`fish`: A Tutorial For Beginners 

为了让我的编程小白朋友也能享受到现代shell的便利之处, 也为了以后少踩点坑, 故把在Windows上使用`fish`(以及POSIX-兼容的运行环境)的安装配置记录于此.

### MSYS2
如果用习惯了Linux上的POSIX兼容的命令行工具, 在切换到CMD/Powershell的编码风格无疑是痛苦的, 经典的方案是MinGW/Cygwin这样的转换工具, 这里我们使用一个比较新的类MinGW工具[`MSYS2`](https://www.msys2.org), 它不仅包含了MinGW, 也有自己的一套Linux虚拟环境, 还从著名的Arch Linux借用了包管理器`pacman`, 个人感觉比MinGW的包管理器(类`apt`)好用不是一点点, 也有国内的镜像可供选择.

#### MSYS2: Installation
1. [`MSYS2`](https://www.msys2.org)首页上就有下载链接, 直接下载安装即可.
2. 安装后就可以在开始菜单里找到MSYS2虚拟环境了(还附送了MinGW32/64)
3. 但是谁会喜欢黑底白字还是宋体的英文呢? 建议配合Windows Terminal食用, [这里](https://www.msys2.org/docs/terminals/)有和WT配合的文档, 具体来讲, 在Windows Terminal里的设置中如下的修改, 不用重启WT就能在其中新建MSYS2标签页了!
    ```JSON
    "defaultProfile": "{17da3cac-b318-431e-8a3e-7fcdefe6d114}",
    "profiles": {
    "list":
    [
        // ...(你原来的shells, 如CMD/PS)
        {
        "guid": "{17da3cac-b318-431e-8a3e-7fcdefe6d114}",
        "name": "MINGW64 / MSYS2",
        "commandline": "C:/msys64/msys2_shell.cmd -defterm -here -no-start -mingw64",
        "startingDirectory": "C:/msys64/home/%USERNAME%",
        "icon": "C:/msys64/mingw64.ico",
        "fontFace": "Lucida Console",
        "fontSize": 9
        },
        {
        "guid": "{2d51fdc4-a03b-4efe-81bc-722b7f6f3820}",
        "name": "MINGW32 / MSYS2",
        "commandline": "C:/msys64/msys2_shell.cmd -defterm -here -no-start -mingw32",
        "startingDirectory": "C:/msys64/home/%USERNAME%",
        "icon": "C:/msys64/mingw32.ico",
        "fontFace": "Lucida Console",
        "fontSize": 9
        },
        {
        "guid": "{71160544-14d8-4194-af25-d05feeac7233}",
        "name": "MSYS / MSYS2",
        "commandline": "C:/msys64/msys2_shell.cmd -defterm -here -no-start -msys",
        "startingDirectory": "C:/msys64/home/%USERNAME%",
        "icon": "C:/msys64/msys2.ico",
        "fontFace": "Lucida Console",
        "fontSize": 9
        },
        // ...
    ]
    }
    ```
4. 现在你就可以在WT中打开并使用MSYS2了! Give it a try!
5. Note: 在MSYS2(MinGW同样)中, 你的原磁盘文件都会被翻译成Linux风格的路径!
   - e.g.: `C:\path\to\file => /c/path/to/file` 

### `fish` Shell

`fish` 是一个现代化的用户友好的shell, 总之就是脚踢`bash`, 拳打`zsh`!

#### `fish` Installation
1. 为了更快的获得软件包, 建议设置`pacman`镜像, 可以使用清华的[TUNA镜像](https://mirrors.tuna.tsinghua.edu.cn/help/msys2/)或者是BFSU的TUNA镜像的[镜像](https://mirrors.bfsu.edu.cn/help/msys2/)
   - 简而言之,就是在pacman的mirror list里加入对应的镜像源
    `Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/msys/$arch`
    并且刷新`pacman`缓存:
    `pacman -Sy`
2. 安装`fish`
    ```shell
    > pacman -S fish
    ```
5. 将默认的shell从`bash`改成`fish`, 修改MSYS2的启动脚本, 它应该在`.../msys64安装路径/msys2_shell.cmd`这个位置:
    ```powershell
    set "LOGINSHELL=fish" # 原来是bash
    ```
6. 并且你可能会想把Windows原来的PATH导入到Linux环境里, 同样修改上述启动脚本:
    ```powershell
    set MSYS2_PATH_TYPE=inherit # 原来是被注释的
    ```
3. 安装`git`, 这一步可能是可选的, 因为`oh-my-fish`可能需要非Windows的git实现.
    ```shell
    > pacman -S git
    ``` 
    - 进一步,你可能需要配置`git`的全局代理, 否则无法clone repository:
        ```shell
        > git config --global http.proxy yourproxy:port
        ```  
4. 安装[`oh-my-fish`](https://github.com/oh-my-fish/oh-my-fish), 一个`fish`的简易包管理器
    ```shell
    > curl -L https://get.oh-my.fish | fish
    ``` 
7. 你基本上配置好了`fish`了! 现在你有了`omf`(即oh-my-fish的缩写), 可以用来安装一些主题和插件!
    ```shell
    > omf install agnoster
    > omf theme agnoster
    ```