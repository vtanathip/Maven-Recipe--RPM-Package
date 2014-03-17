## Maven-Recipe--RPM-Package ##

Maven Recipe: RPM Package

## Installation ##
1. Download cygwin [Download link](http://www.cygwin.com/install.html)
	- choose `rpm,rpmbuild` package to insall

## How to run? ##
- set `cygwin_bin_path` = c:\cygwin\bin or path that you install cygwin
- add path to environment variables path
- create file name `rpmbuild.bat` in `c:\cygwin\bin` and copy this script to it
> 
> SETLOCAL
> PUSHD .
>  
> REM Update buildroot path
> FOR /F "tokens=*" %%i in ('cygpath %3') do SET NEW_BUILDROOT=%%i
>  
> REM Update topdir path
> SET TOPDIR=%5
> SET TOPDIR=%TOPDIR:~9,-1%
> FOR /F "tokens=*" %%i in ('cygpath "%TOPDIR%"') do SET NEW_TOPDIR=%%i
>  
> REM Replace path in spec-file
> SET OLD_PATH=%TOPDIR:\=\\%
> SET NEW_PATH=%NEW_TOPDIR:/=\/%
>  
> REM Original sed command
> REM sed -s -i -e s/%OLD_PATH%\\/%NEW_PATH%\//g %8
>  
> REM replace \ with / i.e. escape \\ replace with escape \/
> sed -s -i -e s/\\/\//g %8
>  
> REM Execute rpmbuild
>  
> bash -c "rpmbuild %1 %2 %NEW_BUILDROOT% %4 ""_topdir %NEW_TOPDIR%"" %6 "%7" --define ""_build_name_fmt %%{ARCH}/%%{NAME}-%%{VERSION}-%%{RELEASE}.%%{ARCH}.rpm"" %8"
>  
> POPD
> ENDLOCAL

- go to working directory and run with command `mvn clean package`
