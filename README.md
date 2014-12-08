openssl-android
===============

Note that Android 64 bit support (both arm64 and x86_64) does not exist yet in Configure LIST. Would need to wait for OpenSSL to support it or add the necessary item.

Download and setup Android NDK:
https://developer.android.com/tools/sdk/ndk/index.html

Download openssl source from here:
http://www.openssl.org/source/

Build Instructions:
http://wiki.openssl.org/index.php/Android

Short Build Instructions:

#ANDROID_NDK env variable must be set, e.g. :
export ANDROID_NDK=$ANDROID_HOME/android-ndk-r10d

rm -rf openssl-*/
tar zxf openssl-*.tar.gz
. ./setenv-android-$ARCH.sh
cd openssl-*/
./config shared -no-ssl2 -no-ssl3 -no-comp -no-hw -no-engine --openssldir=`pwd`/out/$MACHINE
# modify Makefile change -O3 for -Os http://stackoverflow.com/questions/24204366/how-to-build-openssl-as-unversioned-shared-lib-for-android
make depend
make all



Notes:
#./Configure android shared -no-ssl2 -no-ssl3 -no-comp -no-hw -no-engine --openssldir=`pwd`/out/$MACHINE -Wa,--noexecstack
#./Configure android-armv7 shared -no-ssl2 -no-ssl3 -no-comp -no-hw -no-engine --openssldir=`pwd`/out/$MACHINE -Wa,--noexecstack
#./Configure android-x86 shared -no-ssl2 -no-ssl3 -no-comp -no-hw -no-engine --openssldir=`pwd`/out/$MACHINE -Wa,--noexecstack
#./Configure android-x86_64 shared -no-ssl2 -no-ssl3 -no-comp -no-hw -no-engine --openssldir=`pwd`/out/$MACHINE -Wa,--noexecstack #android-x86_64 DOES NOT EXIST YET
#./Configure android-arm64v8 shared -no-ssl2 -no-ssl3 -no-comp -no-hw -no-engine --openssldir=`pwd`/out/$MACHINE -Wa,--noexecstack #android-arm64v8 DOES NOT EXIST YET


Configure LIST for Android: 
"android","gcc:-mandroid -I\$(ANDROID_DEV)/include -B\$(ANDROID_DEV)/lib -O3 -fomit-frame-pointer -Wall::-D_REENTRANT::-ldl:BN_LLONG RC4_CHAR RC4_CHUNK DES_INT DES_UNROLL BF_PTR:${no_asm}:dlfcn:linux-shared:-fPIC::.so.\$(SHLIB_MAJOR).\$(SHLIB_MINOR)",
"android-x86","gcc:-mandroid -I\$(ANDROID_DEV)/include -B\$(ANDROID_DEV)/lib -O3 -fomit-frame-pointer -Wall::-D_REENTRANT::-ldl:BN_LLONG ${x86_gcc_des} ${x86_gcc_opts}:".eval{my $asm=${x86_elf_asm};$asm=~s/:elf/:android/;$asm}.":dlfcn:linux-shared:-fPIC::.so.\$(SHLIB_MAJOR).\$(SHLIB_MINOR)",
"android-armv7","gcc:-march=armv7-a -mandroid -I\$(ANDROID_DEV)/include -B\$(ANDROID_DEV)/lib -O3 -fomit-frame-pointer -Wall::-D_REENTRANT::-ldl:BN_LLONG RC4_CHAR RC4_CHUNK DES_INT DES_UNROLL BF_PTR:${armv4_asm}:dlfcn:linux-shared:-fPIC::.so.\$(SHLIB_MAJOR).\$(SHLIB_MINOR)",
