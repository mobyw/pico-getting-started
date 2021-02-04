# Raspberry Pi Pico C/C++ 开发环境配置

## 安装软件和工具链

- [ARM GCC compiler](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads)
- [CMake](https://cmake.org/download/)
- [Visual Studio 2019 生成工具](https://visualstudio.microsoft.com/zh-hans/downloads/)
- [Python 3](https://www.python.org/downloads/windows/)

可选软件：

- [Git](https://git-scm.com/download/win)
- [Visual Studio Code](https://visualstudio.microsoft.com/zh-hans/downloads/)
- [CLion](https://www.jetbrains.com/clion/)

## 环境变量准备

安装工具时下载 `.exe` 或者 `.msi` 可执行文件，安装过程中勾选 `Add to Path` 以及 `Install for all users` 。

若直接下载压缩包或未勾选 `Add to PATH` ，则需手动将安装目录下 `bin` 文件夹添加到 `Path` 。

## 下载 SDK 与示例工程

如果安装了 Git ，可以运行 `Git Bash` ，先通过 `cd` 命令转到文件存放目录 `xxx` ，然后执行以下命令：

``` bash
xxx > git clone -b master https://github.com/raspberrypi/pico-sdk.git
xxx > cd pico-sdk
xxx\pico-sdk > git submodule update --init
xxx\pico-sdk > cd ..
xxx > git clone -b master https://github.com/raspberrypi/pico-examples.git
xxx > setx PICO_SDK_PATH "xxx\pico-sdk" /M
```

即可下载 SDK 与示例工程，也可以手动在Github下载，链接如下：

- [pico-sdk](https://github.com/raspberrypi/pico-sdk)
- [pico-examples](https://github.com/raspberrypi/pico-examples)

下载并解压后将文件夹名后的 `-master` 删除。并设置 `PICO_SDK_PATH` 环境变量为 `pico-sdk` 文件夹。

相关文档查阅：[pico-sdk-doxygen](https://raspberrypi.github.io/pico-sdk-doxygen/)

## 工程生成器

若不想自己手动配置工程，可以使用工程配置脚本 [pico-project-generator](https://github.com/raspberrypi/pico-project-generator) 来生成工程文件。

使用 Git 或直接打包下载文件解压，在文件夹下打开 `Windows Terminal` ，运行：

``` bash
$ > py -3 ./pico_project.py --gui
```

即可打开配置工程的用户界面，设置好工程名称与工程路径，选择需要使用的库文件，添加示例代码，若使用 `VSCode` 则勾选 `Creat VSCode projects` ，点击 `OK` 等待生成。

## 代码编写

编辑工程目录下的 `.c` 文件，以 LED 闪烁为例，代码如下：

``` cpp
#include "pico/stdlib.h"
#define   LED_PIN   25

int main()
{
    gpio_init(LED_PIN);
    gpio_set_dir(LED_PIN, GPIO_OUT);

    while(1)
    {
        gpio_put(LED_PIN, 1);
        sleep_ms(1000);
        gpio_put(LED_PIN, 0);
        sleep_ms(1000);
    }
}
```

## 工程编译

### 命令行方式

运行 `Developer PowerShell for VS 2019` 或 `Developer Command Prompt for VS 2019` ，转到工程目录下的 `build` 文件夹，运行以下命令：

``` bash
$ > cmake ..
$ > nmake
```

即可在 `build` 目录下生成 `.bin .hex .elf .uf2` 等格式的文件。 

## VSCode

使用 `VSCode` 编译工程时，务必通过在 `Developer PowerShell for VS 2019` 或 `Developer Command Prompt for VS 2019` 中输入 `code` 打开 `VSCode` 软件。

安装以下扩展：

- Cortex-Debug (marus25.cortex-debug)
- CMake Tools (ms-vscode.cmake-tool)
- C/C++ (ms-vscode.cpptools)

在设置文件中添加以下内容，其中 `xxx` 为下载目录：

``` json
"cmake.configureEnvironment": {
    "PICO_SDK_PATH": "xxx\\pico-sdk"
},
"cmake.generator": "NMake Makefiles",
```

打开工程文件夹，发现 `.vscode` 文件夹下的 `c_cpp_properties.json` 文件中有提示错误，是由于生成工具是基于 `Linux` 的，所以 `arm-none-eabi-gcc` 的位置有误，手动修改为 `"xxx\\bin\\arm-none-eabi-gcc.exe"` 即可不报错。

将 `.vscode` 文件夹下的 `settings.json` 文件中的以下几行删除：

``` bash
"cmake.statusbar.advanced": {
    "debug": {
        "visibility": "hidden"
    },    "launch" : {
        "visibility": "hidden"
    },
    "build": {
        "visibility": "hidden"
    },
    "buildTarget": {
        "visibility": "hidden"
    },
},
```

保存文件后即可看到下方的 `cmake` 工具栏。

点击工具栏中的 `未选择任何工具包` ，选择 `GCC for arm-none-eabi` 。

点击工具栏中的 `生成` 即可完成项目编译。
