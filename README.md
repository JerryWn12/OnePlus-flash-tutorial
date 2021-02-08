# OnePlus-flash-tutorial

## 注意事项

- 此教程暂只支持 **一加 8** 刷入 **Havoc OS** ，后续（可能）会支持更多机型或 rom，非 **一加 8** 刷入 **Havoc OS** 的请勿轻易使用本教程尝试！！!

- 手机变砖不要慌， ~~拿出手机拍个朋友圈儿\~\~~~ ，搞机有风险，刷机需谨慎。

## 事前准备

1. 一台电脑（其实一台已 root 的手机也行，后面有空补上）；
2. 一加 8 手机；
3. 手，当然你用脚也行。

## 一， 解锁 Bootloader

\* 此阶段可选，若已经解锁 Bootloader 则可跳过此步，到第三步：备份指纹分区，已解锁 Bootloader 的明显特征是手机在开机时会有设备已解锁的提示。

解锁 Bootloader 是怎么回事呢？解锁 Bootloader 相信大家都很熟悉，但是解锁 Bootloader 是怎么回事呢，下面就让小编带大家一起了解吧。<br>解锁 Bootloader，其实就是用 adb 工具解锁 Bootloader，大家可能会很惊讶 adb 工具怎么会解锁 Bootloader 呢？但事实就是这样，小编也感到非常惊讶。<br>这就是关于解锁 Bootloader 的事情了，大家有什么想法呢，欢迎在评论区告诉小编一起讨论哦！

咳咳，回到正题，下面是谷歌官方对 Bootloader 的介绍：

> **引导加载程序**
>
> 引导加载程序是供应商专有的映像，负责在设备上启动内核。它会监护设备状态，负责初始化 [可信执行环境 (TEE)](https://source.android.com/security/trusty) 以及绑定其信任根。
>
> 引导加载程序由许多部分组成，包括启动画面。要开始启动，引导加载程序可能会直接将一个新映像刷写到相应的分区中，也可能会使用 `recovery` 开始重新刷写过程，该过程与 OTA 的操作过程一致。一些设备制造商会创建多部分引导加载程序，然后将它们组合到一个 bootloader.img 文件中。在刷写时，引导加载程序会提取各个引导加载程序并刷写所有这些引导加载程序。
>
> 最重要的是，引导加载程序会在将执行工作移到内核之前先验证 boot 分区和 recovery 分区的完整性，并显示[启动状态](https://source.android.com/security/verifiedboot/verified-boot#boot_state)部分中指定的警告。
>
> [Source](https://source.android.com/devices/bootloader)

看到最后一段我们知道，引导加载程序（Bootloader）会验证 `boot` 分区和 `recovery` 分区的完整性，而 Magisk Root 就是通过修改 `Boot` 镜像来达到目的的，所以我们只要解锁了 Bootloader 就可以尽情玩耍啦！

那么我们要如何解锁 Bootloader 呢？其实只需要一条命令就行啦：

    fastboot oem unlock

[Source](https://source.android.com/devices/bootloader/locking_unlocking)

> 但是，我查阅谷歌官方文档却是这样说道：2014 年及其之前生产的设备使用上述命令，2015 年
> 及其之后生产的设备使用这条命令：
>
>       fastboot flashing unlock
>
> 关于两者的区别，我也翻阅了许多资料，还是没能总结出这两条命令的功能的不同之处，希望知道两者区别的大佬帮忙 **contribute** 一下，谢谢啦！（我也不懂 Github 怎么贡献代码 T_T

那么，我们应该到哪里输入命令？那么这时候就要介绍一个谷歌官方的 _Android SDK Platform-Tools_ 啦！这也是我们俗称的 **adb** 工具，里面集成了 **adb** 和 **fastboot** 命令，点击下面的任一链接即可下载

- [官方链接（需要科学上网)](https://developer.android.com/studio/releases/platform-tools)
- [蓝奏云](https://jerrywn.lanzous.com/iA9jLlfseyh)

下载完之后解压，打开解压后的文件夹，我们会看见这些：

![解压后文件夹](./assets/屏幕截图%202021-02-07%20170353.png)

使用 adb 工具有多种方法，我介绍其中两种：

### 方法一 ：配置环境变量后随地使用

这些文件可以存放在任意地方 **（路径最好不要有中文）** ，将整个文件夹剪切至任意你想要的位置之后，点击文件资源管理器的地址栏，将路径复制下来，例如我的图片上的路径就是：

    D:\Download\Compressed\platform-tools_r30.0.5-windows\platform-tools

注意路径结尾一定是 `platform-tools` 除非你更改了解压后的文件夹名称。

然后我们需要对 **_环境变量_** 进行配置：Windows10 系统中，使用快捷键 `Windows + S` 打开搜索工具，键入 `PATH` 点击最佳匹配中的 **“编辑系统环境变量”** ，如下图所示：

![搜索](./assets/屏幕截图%202021-02-07%20222601.png)

在打开的窗口中选择 **“环境变量”** 一项，再找到下面那栏系统变量中的变量名为 **Path** 的项，如下图所示：

![环境变量](<./assets/屏幕截图(18).png>)

双击这个 **Path** 项，在打开的窗口中点击右侧的 **新建** ，然后在输入框中粘贴刚刚复制的文件夹地址，点击确定，如下图所示：

![新建Path项](<assets/屏幕截图(19).png>)

此时环境变量搭建完成。

使用快捷键 `Windows + R` 打开运行窗口，键入 `cmd` 回车打开 cmd 窗口，键入：

    fastboot help

此时应出现 fastboot 的相关命令用法及其解释：

![fastboot help](./assets/屏幕截图%202021-02-07%20181840.png)

若出现的是形如：

    'fastboot' 不是内部或外部命令，也不是可运行的程序或批处理文件。

则应检查环境变量有没有正确配置完成。若仍无效，请尝试使用方法二 。

### 方法二 ：就地使用

在解压后的文件夹里面，按住 **Shift** 键在文件夹内空白处右键，菜单里面就会增加 **“在此处打开 PowerShell 窗口”** 一项，如下图所示：

![右键菜单](./assets/屏幕截图%202021-02-07%20225901.png)

选择此项，即可打开命令行窗口，同样地，键入：

    fastboot help

此时应出现 fastboot 的相关命令用法及其解释：

![fastboot help](assets/屏幕截图%202021-02-07%20230403.png)

若仍无效，请尝试使用方法一 。

### 至此，解锁 Bootloader 阶段完成

## 二，打包 Boot，使用 MagiskRoot

\* 此阶段可选，若会打包 Boot 后刷入以使用 Magisk 则可跳过此步，到第三步：备份指纹分区。

既然我们已经解锁了 Bootloader ，那就意味着我们可以对 `Boot` 分区进行修改啦！那我们首先得把 Boot.img 文件提取出来，Boot.img 文件是在 Payload.bin 的解压文件里面，而 Payload.bin 则可以通过解压固件包获得，也就是说，我们需要：

1. 解压固件包获得 Payload.bin 文件
2. 解压 Payload.bin 文件获得 Boot.img 文件

那么这样，我们就可以成功提取 Boot.img 文件啦

这里，我使用一加官方氧固件包 11.0.4.4 进行演示：

选中固件包进行解压，解压完成后我们会看到以下文件和文件夹：

- META-INF (文件夹)
- **Payload.bin**
- payload_properties.txt

![固件包解压文件](./assets/屏幕截图%202021-02-08%20143326.png)

在这里，Payload.bin 就是我们需要的文件啦！再次选中 Payload.bin 文件进行解压，这时发现：

![解压错误](assets/屏幕截图%202021-02-08%20144541.png)

出现了 `有效数据包含额外数据` 的警告，这个警告是怎么回事呢，其实我也不知道，那么在这里我们需要用到一个工具：**Payload Dumper** ，所谓 Payload Dumper，就是专门解压 Payload.bin 的工具啦！在下面的链接可以下载：

- [蓝奏云](https://jerrywn.lanzous.com/isZxKlh0yli)
- [Android File Host](https://androidfilehost.com/?fid=818070582850510260)

下载完成后解压我们得到一个文件夹：`payload_dumper-win64` ，进入文件夹后如下图：

![PayloadDumper](assets/屏幕截图%202021-02-08%20163420.png)

还记得我们解压固件包得到的 **Payload.bin** 文件吗？在这里：

- `payload_input` 就是存放 Payload.bin 文件的地方，
- `payload_output` 就是解压 Payload.bin 文件后的解压文件存放的地方。

那么把 Payload.bin 文件放进 `payload_input` 文件夹里面，双击 `payload_dumper.exe` 就可以把 Boot.img 解压出来啦！

所以步骤是：

1. 把 Payload.bin 粘贴至 `payload_input` 文件夹里面；
2. 双击 `payload_dumper.exe` 等待解压完成；

   > 在这一步，其实不用等待整个 Payload.bin 文件解压完毕，除非你需要除 Boot.img 之外的其他 img 文件，只要出现 Boot 字样，并且 Boot 字样下面还有除零之外的任意个其它内容，即可关闭窗口结束解压进程，如下图所示：
   >
   > ![提前结束](assets/屏幕截图%202021-02-08%20164837.png)
   >
   > 此时整个 Payload.bin 文件并未解压完毕，也可以关闭窗口

3. 在 `payload_output` 文件夹里面即可找到 Boot.img 。

英语好的也可以阅读 Readme.txt 查看用法和注意事项。

Boot.img 文件提取出来了，接下来我们就要打包 Boot 啦！在手机上安装 Magisk 应用，把电脑上解压出来的 Boot.img 文件传输到手机上，打开 Magisk ，选择上面的 `“安装”` ，注意一定是 **上面** 的安装按钮，也就是 `Magisk` 的按钮，而不是 `Manager` 的按钮，如下图所示（界面可能因为版本或安装与否而不同）：

![安装按钮](assets/Screenshot_20210208-170522_Magisk_Manager.png)

然后选择 **“选择并修补一个文件”** 一项，如下图所示（界面可能因为版本或安装与否而不同）：

![选择项](assets/Screenshot_20210208-170610_Magisk_Manager.png)

点击后，会弹出文件选择器，选择刚刚传输至手机上的 Boot.img 文件，选择完成后，会自动回到 Magisk 应用界面，此时右上角 `开始` 按钮变为可点击状态， 如下图所示（界面可能因为版本或安装与否而不同）：

![点击安装](assets/Screenshot_20210208-170624_Magisk_Manager.png)

点击开始后，会自动对 Boot.img 文件进行打包，打包完成后，会自动在原 Boot.img 文件的所在文件夹创建一个新的 Boot_magisk_patched_xxx.img 文件，如下图所示：

![打包过程](assets/Screenshot_20210208-170638_Magisk_Manager.png)

![前后Boot对比](assets/Screenshot_20210208-170656_文件极客.png)

大功告成，我们已经获得了被 Magisk 修改过的 Boot.img 文件，那么接下来，我们就要把 Boot.img 刷入手机，获得 Root 权限。
