
# Windows x64下编译


>bash
>./configure --with-target-bits=64 --with-boot-jdk=/cygdrive/c/work/java/jdk1.7.0_17 --with-freetype="/cygdrive/c/work/freetype-2.9.1" --with-boot-jdk-jvmargs="-Xmx8G -enableassertions"
>make all

# 修改编译信息

/openjdk/common/autoconf/version-numbers
/openjdk/hotspot/make/openjdk_distro

# FAQ 

##  Windows X64
###  Target CPU mismatch?

修改/openjdk/common/autoconf/generated-configure.sh
找到COMPILER_CPU_TEST=`$ECHO $COMPILER_VERSION_TEST | $SED -n "s/^.* for \(.*\)$/\1/p"`
>修改为实际的BITS
COMPILER_CPU_TEST="x64"

###  Your cygwin is too old

修改/openjdk/common/autoconf/generated-configure.sh
CYGWIN_VERSION_OK=`$ECHO $CYGWIN_VERSION | $GREP ^1.7.`
修改为实际的cygwin版本号
CYGWIN_VERSION_OK=`$ECHO $CYGWIN_VERSION | $GREP ^2.11.`


make[3]: *** [Makefile:217：generic_build2] 错误 2
make[2]: *** [Makefile:167：debug] 错误 2
make[1]: *** [HotspotWrapper.gmk:45：/cygdrive/e/hub/openjdk/jdk8u/build/windows-x86_64-normal-server-release/hotspot/_hotspot.timestamp] 错误 2
make: *** [/cygdrive/e/hub/openjdk/jdk8u//make/Main.gmk:109：hotspot-only] 错误 2

因为是cvtres.exe版本错误导致的结果，所以凡是能使VS链接器找到正确的cvtres.exe版本的方法都可以解决该问题。或者使VS链接器不生成COFF的方法都可以。
解决方法：
当前系统中存在两个cvtres.exe文件，版本不同。让VS2010使用.NET 4.5的cvtres.exe程序。
具体步骤：
重命名cvtres.exe为cvtres.exe.bak：（vs2010安装的位置）C:\Program Files (x86)\Microsoft Visual Studio 10.0\VC\bin\cvtres.exe
C:\Program Files (x86)\Microsoft Visual Studio 10.0\VC\bin\amd64\cvtres.exe
这样C:\Windows\Microsoft.NET\Framework\v4.0.30319 (.NET 4.5)中的cvtres.exe文件就可以被VS2010使用。

参考：http://stackoverflow.com/questions/10888391/link-fatal-error-lnk1123-failure-during-conversion-to-coff-file-invalid-or-c

### _the.rt.jar.contents  CreateJars.gmk 格式异常

Updating images/src.zip
make[2]: *** [CreateJars.gmk:268：/cygdrive/e/hub/openjdk/jdk8u/build/windows-x86_64-normal-server-release/images/lib/_the.rt.jar.contents] 错误 1
make[2]: *** 正在等待未完成的任务....
make[1]: *** [BuildJdk.gmk:101：images] 错误 2
make: *** [/cygdrive/e/hub/openjdk/jdk8u//make/Main.gmk:136：images-only] 错误 2

解决方法：

用vi打开/jdk/make目录下的CreateJars.gmk
/openjdk/jdk/make/CreateJars.gmk
定位到268行，相距不远处有两个$$换行符，将其转换为Windows下的换行符。
在VI下，可输入268gg。
将光标定位到两个$$之前，按i切换到insert模式后，按Ctrl + V, Ctrl + M，即可打出^M。
完成后按esc退出编辑模式，然后按:进入命令模式，输入wq保存并退出。

### E:\plugins\openjdk\hotspot/make/windows/get_msc_ver.sh: 第 69 行

运行cl.exe查看版本
/openjdk/hostpot/make/windows/get_msc_ver.sh，65-73行注释掉，直接添加
MSC_VER=1600
MSC_VER_RAW=16.00.30319.01