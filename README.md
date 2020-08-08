# build_apache_on_windows
# How to build Apache HTTPD

## 1 For Windows with VS2017

### 1.1 Install building tools

Before building apache, ensure to download and install these tools in the build environment to the default place.

**[ For WIN 32 bit ]**

- perl http://downloads.activestate.com/ActivePerl/releases/5.24.1.2402/ActivePerl-5.24.1.2402-MSWin32-x86-64int-401627.exe

- cmake https://cmake.org/files/v3.8/cmake-3.8.0-win32-x86.msi

**[ For WIN 64 bit ]**

- Perl64 http://downloads.activestate.com/ActivePerl/releases/5.24.2.2403/ActivePerl-5.24.2.2403-MSWin32-x64-403863.exe

- cmake64 https://cmake.org/files/v3.10/cmake-3.10.0-win64-x64.msi

### 1.2 Build the prerequisite third parties

As apache has dependencies on some other third party software, make sure to compile these softwares beforehand.

Before starting the building process, start a Visual Studio Command Prompt 2010 in a workspace (e.g. D:\P4\BUSMB_B1\SBO\9.2_DEV) and set environment variables as below:

**[ For WIN 32 bit ]**

```
set path=C:\Program Files (x86)\CMake\bin;C:\Perl\bin;%path%
set workspace=D:\my_workspace
```

**[ For WIN 64 bit ]**

```
set path=C:\Program Files\CMake\bin;C:\Perl64\bin;%path%
set workspace=D:\P4\BUSMB_B1\SBO\9.2_DEV
```

#### 1.2.1 Build OpenSSL

1. Sync **openssl-1.0.2k.zip** or **openssl-1.0.2r.zip** from Perforce to your workspace and extract it to the directory %workspace%\Source\ThirdParty\OpenSSL\src\openssl
2. Navigate to the directory and generate the VS2017 solution with cmake.

**[ For WIN 32 bit ]**

```
cd %workspace%\Source\ThirdParty\OpenSSL\src\openssl
perl Configure VC-WIN32 no-asm no-rc5 --prefix=%workspace%\Source\ThirdParty\OpenSSL\WIN32
ms\do_ms
nmake -f ms\ntdll.mak
nmake -f ms\ntdll.mak install
```

**[ For WIN 64 bit ]**

```
cd %workspace%\Source\ThirdParty\OpenSSL\src\openssl
perl Configure VC-WIN64A no-asm no-rc5 --prefix=%workspace%\Source\ThirdParty\OpenSSL\WIN64
ms\do_win64a
nmake -f ms\ntdll.mak
nmake -f ms\ntdll.mak install
```

More details, please see: http://blog.csdn.net/henter/article/details/8364532

**[Note]** There is a different way to build the OpenSSL 1.1.0 version. More details, see

- http://p-nand-q.com/programming/windows/building_openssl_with_visual_studio_2013.html
- https://github.com/openssl/openssl/blob/master/INSTALL#L44

And it seems the current Apache 2.4.27 cannot be compiled against OpenSSL 1.1.0. About the security issue, please see: https://www.cvedetails.com/vulnerability-list/vendor_id-217/product_id-383/version_id-213672/Openssl-Openssl-1.1.0d.html

#### 1.2.2 Build zlib

1. Sync zlib-1.2.11.zip from Perforce to your workspace and extract it to the directory %workspace%\Source\ThirdParty\Apache\src\zlib

2. Navigate to the directory and generate the VS2017 solution with cmake

**[ For WIN 32 bit ]**

```
cd %workspace%\Source\ThirdParty\Apache\src\zlib
cmake -G "Visual Studio 15 2017" -DCMAKE_INSTALL_PREFIX=..\..\WIN32 -DCMAKE_CONFIGURATION_TYPES=Debug;Release
```

**[ For WIN 64 bit ]**

```
cd %workspace%\Source\ThirdParty\Apache\src\zlib
cmake -G "Visual Studio 15 2017 Win64" -DCMAKE_INSTALL_PREFIX=..\..\WIN64 -DCMAKE_CONFIGURATION_TYPES=Debug;Release
```

3. Build the solution with Visual Studio

```
devenv "zlib.sln" /Build Release /project "INSTALL.vcxproj" /projectconfig Release
```

#### 1.2.3 Build pcre

1. Sync pcre-8.39.zip from Perforce to your workspace and extract it to the directory %workspace%\Source\ThirdParty\Apache\src\pcre

2. Navigate to the directory and generate the VS2017 solution with cmake

**[ For WIN 32 bit ]**

```
cd %workspace%\Source\ThirdParty\Apache\src\pcre
cmake -G "Visual Studio 15 2017" -DCMAKE_INSTALL_PREFIX=..\..\WIN32 -DCMAKE_CONFIGURATION_TYPES=Debug;Release -DBUILD_SHARED_LIBS=ON
```

**[ For WIN 64 bit ]**

```
cd %workspace%\Source\ThirdParty\Apache\src\pcre
cmake -G "Visual Studio 15 2017 Win64" -DCMAKE_INSTALL_PREFIX=..\..\WIN64 -DCMAKE_CONFIGURATION_TYPES=Debug;Release -DBUILD_SHARED_LIBS=ON
```

3. Build the solution with Visual Studio

```
devenv "PCRE.sln" /Build Release /project "INSTALL.vcxproj" /projectconfig Release
```

#### 1.2.4 Build expat

1. Sync expat-2.2.0.zip from Perforce to your workspace and extract it to the directory %workspace%\Source\ThirdParty\Apache\src\expat

2. Navigate to the directory and generate the VS2017 solution with cmake

**[ For WIN 32 bit ]**

```
cd %workspace%\Source\ThirdParty\Apache\src\expat
cmake -G "Visual Studio 15 2017" -DCMAKE_INSTALL_PREFIX=..\..\WIN32 -DCMAKE_CONFIGURATION_TYPES=Debug;Release
```

**[ For WIN 64 bit ]**

```
cd %workspace%\Source\ThirdParty\Apache\src\expat
cmake -G "Visual Studio 15 2017 Win64" -DCMAKE_INSTALL_PREFIX=..\..\WIN64 -DCMAKE_CONFIGURATION_TYPES=Debug;Release
```

3. Build the solution with Visual Studio

```
devenv "expat.sln" /Build Release /project "INSTALL.vcxproj" /projectconfig Release
```

#### 1.2.5 Build APR

1. Sync apr-1.5.2.zip from Perforce to your workspace and extract it to the directory %workspace%\Source\ThirdParty\Apache\src\apr

2. Navigate to the directory and generate the VS2017solution with cmake

**[ For WIN 32 bit ]**

```
cd %workspace%\Source\ThirdParty\Apache\src\apr
cmake -G "Visual Studio 15 2017" -DCMAKE_INSTALL_PREFIX=..\..\WIN32 -DCMAKE_CONFIGURATION_TYPES=Debug;Release -DAPR_INSTALL_PRIVATE_H=ON
```

**[ For WIN 64 bit ]**

```
cd %workspace%\Source\ThirdParty\Apache\src\apr
cmake -G "Visual Studio 15 2017 Win64" -DCMAKE_INSTALL_PREFIX=..\..\WIN64 -DCMAKE_CONFIGURATION_TYPES=Debug;Release -DAPR_INSTALL_PRIVATE_H=ON
```

3. Build the solution with Visual Studio

```
devenv "APR.sln" /Build Release /project "INSTALL.vcxproj" /projectconfig Release
```

#### 1.2.6 Build APR-Util

1. Sync apr-util-1.5.4.zip from Perforce to your workspace and extract it to the directory %workspace%\Source\ThirdParty\Apache\src\apr-util

2. Navigate to the directory and generate the VS2017solution with cmake

**[ For WIN 32 bit ]**

```
cd %workspace%\Source\ThirdParty\Apache\src\apr-util
cmake -G "Visual Studio 15 2017" -DCMAKE_INSTALL_PREFIX=..\..\WIN32 -DCMAKE_CONFIGURATION_TYPES=Debug;Release -DLIB_EAY_RELEASE=..\..\..\OpenSSL\WIN32\lib\libeay32.lib -DLIB_EAY_DEBUG=..\..\..\OpenSSL\WIN32\lib\libeay32.lib -DOPENSSL_INCLUDE_DIR=..\..\..\OpenSSL\WIN32\include -DSSL_EAY_RELEASE=..\..\..\OpenSSL\WIN32\lib\ssleay32.lib -DSSL_EAY_DEBUG=..\..\..\OpenSSL\WIN32\lib\ssleay32.lib -DAPR_LIBRARIES=..\..\WIN32\lib\libapr-1.lib;..\..\WIN32\lib\expat.lib
```

**[ For WIN 64 bit ]**

```
cd %workspace%\Source\ThirdParty\Apache\src\apr-util
cmake -G "Visual Studio 15 2017 Win64" -DCMAKE_INSTALL_PREFIX=..\..\WIN64 -DCMAKE_CONFIGURATION_TYPES=Debug;Release -DLIB_EAY_RELEASE=..\..\..\OpenSSL\WIN64\lib\libeay32.lib -DLIB_EAY_DEBUG=..\..\..\OpenSSL\WIN64\lib\libeay32.lib -DOPENSSL_INCLUDE_DIR=..\..\..\OpenSSL\WIN64\include -DSSL_EAY_RELEASE=..\..\..\OpenSSL\WIN64\lib\ssleay32.lib -DSSL_EAY_DEBUG=..\..\..\OpenSSL\WIN64\lib\ssleay32.lib -DAPR_LIBRARIES=..\..\WIN64\lib\libapr-1.lib;..\..\WIN64\lib\expat.lib
```

3. Build the solution with Visual Studio

```
devenv "APR-Util.sln" /Build Release /project "INSTALL.vcxproj" /projectconfig Release
```

## 2 Build Apache

1. Sync httpd-2.4.25.zip from Perforce to your workspace and extract it to the directory %workspace%\Source\ThirdParty\Apache\src\httpd

2. Navigate to the directory and generate the VS2017 solution with cmake

**[ For WIN 32 bit ]**

```
cd %workspace%\Source\ThirdParty\Apache\src\httpd
cmake -G "Visual Studio 15 2017" -DCMAKE_INSTALL_PREFIX=..\..\WIN32 -DCMAKE_CONFIGURATION_TYPES=Debug;Release -DAPR_LIBRARIES=..\..\WIN32\lib\libapr-1.lib;..\..\WIN32\lib\libaprutil-1.lib;..\..\WIN32\lib\apr_ldap-1.lib -DPCRE_LIBRARIES=..\..\WIN32\lib\pcre.lib -DLIB_EAY_RELEASE=..\..\..\OpenSSL\WIN32\lib\libeay32.lib -DLIB_EAY_DEBUG=..\..\..\OpenSSL\WIN32\lib\libeay32.lib -DOPENSSL_INCLUDE_DIR=..\..\..\OpenSSL\WIN32\include -DSSL_EAY_RELEASE=..\..\..\OpenSSL\WIN32\lib\ssleay32.lib -DSSL_EAY_DEBUG=..\..\..\OpenSSL\WIN32\lib\ssleay32.lib
```

**[ For WIN 64 bit ]**

```
cd %workspace%\Source\ThirdParty\Apache\src\httpd
cmake -G "Visual Studio 15 2017 Win64" -DCMAKE_INSTALL_PREFIX=..\..\WIN64 -DCMAKE_CONFIGURATION_TYPES=Debug;Release -DAPR_LIBRARIES=..\..\WIN64\lib\libapr-1.lib;..\..\WIN64\lib\libaprutil-1.lib;..\..\WIN64\lib\apr_ldap-1.lib -DPCRE_LIBRARIES=..\..\WIN64\lib\pcre.lib -DLIB_EAY_RELEASE=..\..\..\OpenSSL\WIN64\lib\libeay32.lib -DLIB_EAY_DEBUG=..\..\..\OpenSSL\WIN64\lib\libeay32.lib -DOPENSSL_INCLUDE_DIR=..\..\..\OpenSSL\WIN64\include -DSSL_EAY_RELEASE=..\..\..\OpenSSL\WIN64\lib\ssleay32.lib -DSSL_EAY_DEBUG=..\..\..\OpenSSL\WIN64\lib\ssleay32.lib
```

3. Build the solution with Visual Studio

```
devenv "HTTPD.sln" /Build Release /project "INSTALL.vcxproj" /projectconfig Release
```

**[Note]** If `ApacheMonitor` is not built, make the following file changes so that `ApacheMonitor` gets built without the Manifest error:

- Uncomment the below section to build the ApacheMonitor utility in CMakeLists.txt

  ```
  # getting duplicate manifest error with ApacheMonitor 
  ADD_EXECUTABLE(ApacheMonitor support/win32/ApacheMonitor.c support/win32/ApacheMonitor.rc) 
  SET(install_targets ${install_targets} ApacheMonitor) 
  SET(install_bin_pdb ${install_bin_pdb} ${PROJECT_BINARY_DIR}/ApacheMonitor.pdb) 
  SET_TARGET_PROPERTIES(ApacheMonitor PROPERTIES WIN32_EXECUTABLE TRUE) 
  SET_TARGET_PROPERTIES(ApacheMonitor PROPERTIES COMPILE_FLAGS "-DAPP_FILE -DLONG_NAME=ApacheMonitor -DBIN_NAME=ApacheMonitor.exe / ${EXTRA_COMPILE_FLAGS}") 
  TARGET_LINK_LIBRARIES(ApacheMonitor ${EXTRA_LIBS} ${HTTPD_SYSTEM_LIBS} comctl32 wtsapi32)
  ```

- Comment out the line that includes ApacheMonitor.manifest in ApacheMonitor.rc.

  ```
  //CREATEPROCESS_MANIFEST_RESOURCE_ID RT_MANIFEST  "ApacheMonitor.manifest"
  ```
