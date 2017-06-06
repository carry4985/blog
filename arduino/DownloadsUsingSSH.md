# 远程烧录hex文件

绝大多数情况下，我们使用树莓派都是使用SSH连接。当我们需要连接Arduino和树莓派时，常用的连接方式是USB连接。那么我们就不能使用IDE开发Arduino，本篇文章介绍的是在Windows下编译，然后保存hex文件。使用FTP/sabma工具上传到树莓派上，继而使用avrdude烧写到Arduino上. 

1. 在Windows下编译Arduino，保存*.hex文件 
2. Arduino 编译环境介绍 

	Arduino使用的是 avr-lib, avr-gcc 和avrdude，IDE配合使用processing（基于Java）。avr-lib是avr编程底层函数库，avr-gcc是编译器，avrdude是下载工具。在raspberry pi 上，就只需要完成下载的工作，而下载的工作只需要avrdude。 
3. avrdude 

	avrdude的参数部分，包括了烧写arduino所需要的所有参数，包括芯片型号、下载器、端口号、传输速率、flash(经由avr-gcc编译而得的hex文件)。
	
		mcu=atmega8 
		f_cpu=16000000 
		format=ihex 
		rate=19200 
		port=/dev/ttyusb0 
		programmer=stk500 
		target_file=test.hex 
		avrdude -F -p mcu?Pport -c programmer?brate -Uflash:w:$target_file 
		ls /dev 
	Arduino位于/dev/ttyACM0 

	使用avrdude烧写 
```
avrdude -v -p atmega328p -c arduino -P /dev/ttyUSB0 -b 57600 -D -U flash:w:speedservonew.ino.with_bootloader.eightanaloginputs.hex
```