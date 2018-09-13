#FAQ 

##Windows X65
###Target CPU mismatch?

修改generated-configure.sh
找到COMPILER_CPU_TEST=`$ECHO $COMPILER_VERSION_TEST | $SED -n "s/^.* for \(.*\)$/\1/p"`
>修改为实际的BITS
COMPILER_CPU_TEST="x64"

###Your cygwin is too old

修改generated-configure.sh
CYGWIN_VERSION_OK=`$ECHO $CYGWIN_VERSION | $GREP ^1.7.`
修改为实际的cygwin版本号
CYGWIN_VERSION_OK=`$ECHO $CYGWIN_VERSION | $GREP ^2.11.`
