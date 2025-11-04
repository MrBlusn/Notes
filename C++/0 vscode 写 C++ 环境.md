- 下载安装 vscode
- 下载安装 MinGW- W64（[Release 2025-08-30 · msys2/msys2-installer · GitHub](https://github.com/msys2/msys2-installer/releases/tag/2025-08-30)）
	- 安装完成后，会出现下面的页面 。![[Pasted image 20251005161058.png|450]]
	- 输入命令：
		`pacman -S --needed base-devel mingw-w64-ucrt-x86_64-toolchain`
		![[Pasted image 20251005161429.png|475]]
	- 看到如下选择，直接回车。
		![[Pasted image 20251005162042.png|347]]
	- 当系统提示是否继续安装时，请输入`y`并回车。
		![[Pasted image 20251005162213.png|372]]
	- 当所有的包都安装好后，直接关闭终端。
		![[Pasted image 20251005162306.png|400]]
- 配置  MSYS2 的环境变量
	- 打开安装 MSYS2 的目录，先找到`ucrt64`文件夹并进入，再找到`bin`文件夹并进入，然后在地址栏中，复制路径。
		![[Pasted image 20251005162510.png|575]]
	- 将复制的路径写到环境变量的 Path 中，点击确认。
		![[Pasted image 20251005162930.png|450]]
	- 最后在 cmd 窗口实验是否安装成功
		![[Pasted image 20251005163307.png]]
- 在 vscode 中测试
	![[Pasted image 20251005170401.png|500]]
	