# Arduino中hex文件的保存及应用 

arduino在编译、链接、下载之后，hex文件自动删除了，造成软件仿真（如用proteus仿真）及其他单片机板应用的不便。以下是自己实践的小结，与大家分享。

##  Hex文件的提取 

1. 在arduino工具的File->preferences中找到preferences.txt文件。 
2. 用记事本打开preferences.txt,选择hex文件存放的路径，在最后行加入`build.path=d:\arduino\MyHexDir`,
3. 关闭arduino。 
4. 关闭preferences.txt ，关闭时对话框显示是否保存，选择保存。 

**Note**：

1. hex文件存放的路径可以由自己来定。
2. 以上操作时不连接arduino硬件。

## 仿真时单片机晶振频率的选择 

在arduino软件包的hardware\arduino\bootloaders\atmega路径下有一个makefile的文件，用记事本打开，可以看到相应的arduino板对应用到的bootloader程序和晶振频率。 

在用proteus仿真时，选择相对应的单片机，配置晶振。单片机应该与arduino在编译时选择的board上的一致。 
 
## 往其他单片机板上烧录 

编译得到的Hex文件往其他的单片机板上烧录时也是一样要选择相对应的单片机和晶振频率。

## Hex文件的保存 
建立保存路径后，每次编译的文件都会存在此路径下，所以程序实验OK后，就应该将相应的Hex文件保存到其他地方，以免在编译其他的程序时被覆盖。
