Typical symptom when trying to connect to MySQL / MariaDB using Qt but without the necessary plugin is the runtime error 


<pre><code>
 QSqlDatabase: QMYSQL driver not loaded
 QSqlDatabase: available drivers: QMYSQL
</code></pre>


Since the Qt Company cannot provide qsqlmysql.dll / libqsqlmysql.so in binary form, you have to build it on your own, which can be a pain. Here is a build for various Qt versions. Get precompiled qsqlmysql.dll for Windows / libqsqlmysql.so for Linux from <a href="https://github.com/thecodemonkey86/qt_mysql_driver/releases">releases</a>. <strong>Be sure to match your Qt version and compiler (Microsoft Visual C++/MSVC, MinGW) <i>EXACTLY</i>. For example if you use Qt 6.4.2, you cannot use driver version 6.4.1 or 6.4.3</strong>

This is helpful to you? If you wish, you have the possibility to support me by donating <a href="https://www.paypal.com/donate/?hosted_button_id=2K7H59EFMSRDU">here</a>. Thank you so much everyone who has donated so far

<a href="https://www.paypal.com/donate/?hosted_button_id=2K7H59EFMSRDU"><img src="https://github.com/thecodemonkey86/qt_mysql_driver/assets/11927938/02524397-e7f7-47ca-be6b-7c8d3a3b5b32"></a>

<b>
Latest Qt6 version: Download for Qt 6.10.0 <a href="https://github.com/thecodemonkey86/qt_mysql_driver/releases/tag/qmysql_6.10.0">here</a><br>
For Android see 3rd party repository https://github.com/sayyyed/qt_android_mysql_driver/releases/tag/qt_mysql_driver_for_android
</b>
<br>


<h4>Deployment</h4>

1) copy qsqlmysql.dll (release build) / MSVC: qsqlmysqld.dll, MinGW: qsqlmysql.dll+qsqlmysql.debug (debug build) to subdirectory "sqldrivers" of application directory (or build directory while developing) 
 <img src="https://github.com/thecodemonkey86/qt_mysql_driver/assets/11927938/ad400ff5-04b2-40f0-b9ab-b72f89168ebd"/>

2) copy libmysql.dll (MySQL library) and the libcrypto/libssl OpenSSL libraries from zip file (or from https://dev.mysql.com and https://kb.firedaemon.com/support/solutions/articles/4000121705  respectively) to application directory (or more generally, any directory that is registered in PATH environment variable)

 <img src="https://github.com/thecodemonkey86/qt_mysql_driver/assets/11927938/8a894abf-bd6a-4016-853d-4e210e2c23bb"/>


<hr>
<h4>Building from source (Qt6/CMake)</h4>

<h5>Windows</h5>
for MSVC:

Prerequisites:
1. Install MSVC Compiler (C++ Desktop Toolkit from [Visual Studio 2022 Setup](https://c2rsetup.officeapps.live.com/c2r/downloadVS.aspx?sku=community&channel=Release&version=VS2022&source=VSLandingPage&includeRecommended=true&cid=2030))
2. Install Qt incl. Sources, CMake and Ninja from Maintenance Tool
3. Install MySQL library, https://dev.mysql.com/downloads/file/?id=543699   

```console
set PATH=%PATH%;C:\Qt\Tools\CMake_64\bin;C:\Qt\Tools\Ninja
C:
cd C:\Qt\6.9.2\Src\qtbase\src\plugins\sqldrivers
call "C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Auxiliary\Build\vcvars64.bat"
call C:\Qt\6.9.2\msvc2022_64\bin\qt-cmake.bat -G "Ninja Multi-Config" . -DMySQL_INCLUDE_DIR="C:\Users\Administrator\Documents\qt_creator\lib\libmysql\include" -DMySQL_LIBRARY="C:\Users\Administrator\Documents\qt_creator\lib\libmysql\lib\libmysql.lib" -DCMAKE_INSTALL_PREFIX="C:\Qt\6.9.2\msvc2022_64" -DCMAKE_CONFIGURATION_TYPES=Release;Debug  -DQT_GENERATE_SBOM=OFF
ninja
ninja install
pause
```

for MinGW:

Prerequisites:
1. Install Qt incl. Sources, CMake and MinGW from Maintenance Tool
2. Install MySQL library, https://dev.mysql.com/downloads/file/?id=543699   

```console
set PATH=%PATH%;C:\Qt\Tools\CMake_64\bin;C:\Qt\Tools\mingw1310_64\bin;C:\Qt\Tools\Ninja
C:
cd C:\Qt\6.9.2\Src\qtbase\src\plugins\sqldrivers
call C:\Qt\6.9.2\mingw_64\bin\qt-cmake.bat -G "Ninja Multi-Config" . -DMySQL_INCLUDE_DIR="C:\Users\Administrator\Documents\qt_creator\lib\libmysql\include" -DMySQL_LIBRARY="C:\Users\Administrator\Documents\qt_creator\lib\libmysql\lib\libmysql.lib" -DCMAKE_INSTALL_PREFIX="C:\Qt\6.9.2\mingw_64" -DCMAKE_C_COMPILER="gcc.exe" -DCMAKE_CXX_COMPILER="g++.exe" -DCMAKE_CONFIGURATION_TYPES=Release;Debug   -DQT_GENERATE_SBOM=OFF
ninja
ninja install
pause
```
