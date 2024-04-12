## adb全文(有点长,忍忍……)
```ADB Source Code
LinuxKernel ~ % adb 

Android Debug Bridge version 1.0.41

Version 34.0.5-10900879

Running on Darwin 22.6.0 (arm64)

  

global options:

 -a                       listen on all network interfaces, not just localhost

 -d                       use USB device (error if multiple devices connected)

 -e                       use TCP/IP device (error if multiple TCP/IP devices available)

 -s SERIAL                use device with given serial (overrides $ANDROID_SERIAL)

 -t ID                    use device with given transport id

 -H                       name of adb server host [default=localhost]

 -P                       port of adb server [default=5037]

 -L SOCKET                listen on given socket for adb server [default=tcp:localhost:5037]

 --one-device SERIAL|USB  only allowed with 'start-server' or 'server nodaemon', server will only connect to one USB device, specified by a serial number or USB device address.

 --exit-on-write-error    exit if stdout is closed

  

general commands:

 devices [-l]             list connected devices (-l for long output)

 help                     show this help message

 version                  show version num

  

networking:

 connect HOST[:PORT]      connect to a device via TCP/IP [defaultport=5555]

 disconnect [HOST[:PORT]]

     disconnect from given TCP/IP device [default port=5555], or all

 pair HOST[:PORT] [PAIRING CODE]

     pair with a device for secure TCP/IP communication

 forward --list           list all forward socket connections

 forward [--no-rebind] LOCAL REMOTE

     forward socket connection using:

       tcp:<port> (<local> may be "tcp:0" to pick any open port)

       localabstract:<unix domain socket name>

       localreserved:<unix domain socket name>

       localfilesystem:<unix domain socket name>

       dev:<character device name>

       jdwp:<process pid> (remote only)

       vsock:<CID>:<port> (remote only)

       acceptfd:<fd> (listen only)

 forward --remove LOCAL   remove specific forward socket connection

 forward --remove-all     remove all forward socket connections

 reverse --list           list all reverse socket connections from device

 reverse [--no-rebind] REMOTE LOCAL

     reverse socket connection using:

       tcp:<port> (<remote> may be "tcp:0" to pick any open port)

       localabstract:<unix domain socket name>

       localreserved:<unix domain socket name>

       localfilesystem:<unix domain socket name>

 reverse --remove REMOTE  remove specific reverse socket connection

 reverse --remove-all     remove all reverse socket connections from device

 mdns check               check if mdns discovery is available

 mdns services            list all discovered services

  

file transfer:

 push [--sync] [-z ALGORITHM] [-Z] LOCAL... REMOTE

     copy local files/directories to device

     --sync: only push files that are newer on the host than the device

     -n: dry run: push files to device without storing to the filesystem

     -z: enable compression with a specified algorithm (any/none/brotli/lz4/zstd)

     -Z: disable compression

 pull [-a] [-z ALGORITHM] [-Z] REMOTE... LOCAL

     copy files/dirs from device

     -a: preserve file timestamp and mode

     -z: enable compression with a specified algorithm (any/none/brotli/lz4/zstd)

     -Z: disable compression

 sync [-l] [-z ALGORITHM] [-Z] [all|data|odm|oem|product|system|system_ext|vendor]

     sync a local build from $ANDROID_PRODUCT_OUT to the device (default all)

     -n: dry run: push files to device without storing to the filesystem

     -l: list files that would be copied, but don't copy them

     -z: enable compression with a specified algorithm (any/none/brotli/lz4/zstd)

     -Z: disable compression

  

shell:

 shell [-e ESCAPE] [-n] [-Tt] [-x] [COMMAND...]

     run remote shell command (interactive shell if no command given)

     -e: choose escape character, or "none"; default '~'

     -n: don't read from stdin

     -T: disable pty allocation

     -t: allocate a pty if on a tty (-tt: force pty allocation)

     -x: disable remote exit codes and stdout/stderr separation

 emu COMMAND              run emulator console command

  

app installation (see also `adb shell cmd package help`):

 install [-lrtsdg] [--instant] PACKAGE

     push a single package to the device and install it

 install-multiple [-lrtsdpg] [--instant] PACKAGE...

     push multiple APKs to the device for a single package and install them

 install-multi-package [-lrtsdpg] [--instant] PACKAGE...

     push one or more packages to the device and install them atomically

     -r: replace existing application

     -t: allow test packages

     -d: allow version code downgrade (debuggable packages only)

     -p: partial application install (install-multiple only)

     -g: grant all runtime permissions

     --abi ABI: override platform's default ABI

     --instant: cause the app to be installed as an ephemeral install app

     --no-streaming: always push APK to device and invoke Package Manager as separate steps

     --streaming: force streaming APK directly into Package Manager

     --fastdeploy: use fast deploy

     --no-fastdeploy: prevent use of fast deploy

     --force-agent: force update of deployment agent when using fast deploy

     --date-check-agent: update deployment agent when local version is newer and using fast deploy

     --version-check-agent: update deployment agent when local version has different version code and using fast deploy

     --local-agent: locate agent files from local source build (instead of SDK location)

     (See also `adb shell pm help` for more options.)

 uninstall [-k] PACKAGE

     remove this app package from the device

     '-k': keep the data and cache directories

  

debugging:

 bugreport [PATH]

     write bugreport to given PATH [default=bugreport.zip];

     if PATH is a directory, the bug report is saved in that directory.

     devices that don't support zipped bug reports output to stdout.

 jdwp                     list pids of processes hosting a JDWP transport

 logcat                   show device log (logcat --help for more)

  

security:

 disable-verity           disable dm-verity checking on userdebug builds

 enable-verity            re-enable dm-verity checking on userdebug builds

 keygen FILE

     generate adb public/private key; private key stored in FILE,

  

scripting:

 wait-for[-TRANSPORT]-STATE...

     wait for device to be in a given state

     STATE: device, recovery, rescue, sideload, bootloader, or disconnect

     TRANSPORT: usb, local, or any [default=any]

 get-state                print offline | bootloader | device

 get-serialno             print <serial-number>

 get-devpath              print <device-path>

 remount [-R]

      remount partitions read-write. if a reboot is required, -R will

      will automatically reboot the device.

 reboot [bootloader|recovery|sideload|sideload-auto-reboot]

     reboot the device; defaults to booting system image but

     supports bootloader and recovery too. sideload reboots

     into recovery and automatically starts sideload mode,

     sideload-auto-reboot is the same but reboots after sideloading.

 sideload OTAPACKAGE      sideload the given full OTA package

 root                     restart adbd with root permissions

 unroot                   restart adbd without root permissions

 usb                      restart adbd listening on USB

 tcpip PORT               restart adbd listening on TCP on PORT

  

internal debugging:

 start-server             ensure that there is a server running

 kill-server              kill the server if it is running

 reconnect                kick connection from host side to force reconnect

 reconnect device         kick connection from device side to force reconnect

 reconnect offline        reset offline/unauthorized devices to force reconnect

  

usb:

 attach                   attach a detached USB device

 detach                   detach from a USB device to allow use by other processes

environment variables:

 $ADB_TRACE

     comma/space separated list of debug info to log:

     all,adb,sockets,packets,rwx,usb,sync,sysdeps,transport,jdwp

 $ADB_VENDOR_KEYS         colon-separated list of keys (files or directories)

 $ANDROID_SERIAL          serial number to connect to (see -s)

 $ANDROID_LOG_TAGS        tags to be used by logcat (see logcat --help)

 $ADB_LOCAL_TRANSPORT_MAX_PORT max emulator scan port (default 5585, 16 emus)

 $ADB_MDNS_AUTO_CONNECT   comma-separated list of mdns services to allow auto-connect (default adb-tls-connect)
```
## adb全文大白话翻译(正片开始):
> 其中 \[\]是可选项，非必选
1. 全局选项
	- -a  监听所有网络接口，而不仅仅是本地主机
	- -d  使用USB设备(如果连接多个设备会出错)
	- -e  使用TCP/IP设备(如果多个TCP/IP设备可用，则会出错)
2. 一般命令
	1. devices \[-l\]  列出连接的设备(-l表示长输出)  
		e.g 1. ``` adb devices ```  
		e.g 2. ```adb device -l```
	2. help 输出本帮助信息(将上面的adb命令表打印出来一份)
	3. version 输出adb版本
3. 网络
	1. connect 地址\[:端口\]  通过TCP/IP连接设备\[默认端口=5555\]  
		e.g 1.```adb connect 192.168.1.5```  
		e.g 2.```adb connect 192.168.1.7:8080```
	2. disconnect \[地址\[:端口\]\]  断开指定的设备\[默认端口=5555\]，或者全部断开  
		e.g 1.```adb disconnect```(全部断开)   
		e.g 2.```adb disconnect 192.168.1.5```  
		e.g 3.```adb disconnect 192.168.1.7:8080```  
	3. pair 地址\[:端口\] \[配对代码\]  通过TCP/IP与设备通信并安全配对  
		e.g 1.```adb pair 192.168.1.5```  
		e.g 2.```adb pair 192.168.1.7:8080```  
		e.g 3.```adb pair 192.168.1.7:8080 896453``` 
	4. forward --list  列出所有转发套接字连接(有点透明代理的感觉)  
		e.g:
		```e.g:
		baidaidai@baidaidaidebijibendiannao ~ % adb forward --list
		835282b9 tcp:11111 tcp:22222
		```
	5. forward \[--no-rebind**(重新绑定端口时不要忽略已经绑定的端口)** \] '本地' '远程'  在本地设备和远程设备之间设置端口转发或套接字连接  
		e.g 1.```adb forward tcp:12345 tcp:6789```(纯端口转发)  
		e.g 1.1.```adb forward tcp:0 tcp:6789```(若不关心本地端口值，可设为0让adb自行搜寻可用端口)  
		e.g 1.2.```adb forward --no-rebind tcp:12345 tcp:6789```(若新绑端口与原端口重复，则丢出报错)  
	6. forward --remove tcp:xxx  移除特定标识的正向套接字连接**(移除了其中一个方向，则整个连接都移除)**  
		e.g 1.```adb forward --remove tcp:5555```  
	7. forward --remove-all  移除所有正向套接字连接  
		e.g 1.```adb forward --remove-all```
	8. 懂的都懂，不懂的还是不懂，与上文一样，直接略过
	9. mdns check 检查MDNS发现是否可用  
		e.g 1.```adb mdns check```
	10. mdns services  列出所有发现的mdns服务  
		e.g 1.```adb mdns services```
4. 文件传输
	1. push \[--sync(文件同步)\] \[-z ALGORITHM(压缩算法)\] \[-Z(禁用压缩)\] '本地路径' '远程路径'  复制本地文件/目录到设备  
		e.g 1.```adb push --sync /local /remote```(只推送主机上比设备更新的文件)  
		e.g 2.```adb push -Z /local /remote```(禁用压缩推送文件)  
		e.g 3.```adb push -z lz4 /local /remote```(使用lz4压缩算法推送文件)  
	2. pull \[-a(保留文件时间戳和模式)\] \[-z ALGORITHM(压缩算法)\] \[-Z(禁用压缩)\] '远程路径' '本地路径'   
		e.g 1.```adb pull -a /remote /local```(保留文件时间戳和模式抽取文件)  
		e.g 2.```adb pull -Z /remote /local```(禁用压缩抽取文件)  
		e.g 3.```adb pull -z lz4 /remote /local```(使用lz4压缩算法抽取文件)  
	3. sync \[-l(列出要复制的文件但不复制)\] \[-n(将文件推送到设备，而不存储到文件系统)\]  \[-z ALGORITHM(压缩算法)\] \[-Z(禁用压缩)\] \[PARTITION(指定要同步的分区)\]  将本地构建从设备同步到设备(默认全部)
5. 安装app
	1. install \[-修饰符\] \[--特殊修饰符\] '安装包'  将单个包推送到设备并进行安装  
		e.g 1.`adb install -r /path/to/your/packages`
	2. install-multiple \[-修饰符\] \[--特殊修饰符\] '安装包'  将多个apk推送到设备上进行安装 **(很废，建议多重安装使用下面的)**
	3. install-multi-package \[-修饰符\] \[--特殊修饰符\] '安装包'  将一个或多个包推送到设备并自动安装  
		e.g 1.`adb install-multi-package -g /path/to/frist/package /path/to/second/package`
	4. 修饰符大全
		- -r: 替换现有应用程序
		- -t: 允许测试包
		- -d: 允许降级安装(仅限可调试包)
		- -p: 部分应用程序安装(仅安装多个)
		- -g: 授予所有运行时所需权限
		- --abi ABI: 覆盖平台的默认ABI
		- --instant:使应用程序作为临时安装应用程序安装 
		- \*\*下面的懂的、会的可以继续往下看\*\*
		- --no-streaming:总是将APK推送到设备并调用包管理器作为单独的步骤 
		- --streaming: 强制将APK直接流式传输到软件包管理器中 
		- --fastdeploy:使用快速部署 
		- --no-fastdeploy:禁止使用快速部署 
		- --force-agent:使用快速部署时强制更新部署代理 
		- --date-check-agent:当本地版本较新并使用快速部署时更新部署代理 
		- --version-check-agent:当本地版本有不同的版本代码并使用快速部署时更新部署代理 
		- --local-agent:从本地源代码构建(不是SDK位置)定位代理文件
	5. uninstall \[-k(保留数据和缓存目录)\] '包名'  从设备中删除此应用程序包  
		e.g 1:`adb uninstall -k com.android.apks`
6. 调试
	1. ugreport \[路径\]  
		e.g 1.`adb ugreport /sdcard/debug`
		- 生成设备的 bug 报告，并将其写入指定的路径  
		- 如果不提供路径，则默认为 `bugreport.zip`，并将 bug 报告保存在当前目录中  
		- 如果路径是一个目录，则 bug 报告将保存在该目录中
		- 如果设备不支持压缩的 bug 报告，则输出将发送到标准输出
	3. jdwp  列出承载JDWP传输的进程的pid
		e.g 1.`adb jdwp`
		- 设备上的应用调试模式必须已经启用，并且应用程序也必须已经启动并处于调试状态
	4. logcat  显示设备的日志
		e.g 1.`adb logcat`
7. 安全
	1. disable-verity  在userdebug版本上禁用dm-verity检查  
		e.g 1.`adb disable-verity`
	2. enable-verity  在userdebug版本上重新启用dm-verity检查  
		e.g 1.`在userdebug版本上重新启用dm-verity检查`
	3. keygen FILE  生成adb公私钥，并将私钥存在路径中
8. 撰写脚本
	1. wait-for\[-TRANSPORT\]-STATE  等待设备处于给定状态  
		e.g 1.`adb wait-for-usb-recovery`  
		e.g 2.`adb wait-for-bootloader`  
		- STATE(状态): device, recovery, rescue, sideload, bootloader, 或 disconnect
		- TRANSPORT(传输介质): usb, local, or any \[default=any(默认是任意)\]
	2. get-state  输出 offline | bootloader | devices  
		e.g 1.`adb get-state`
	3. get-serialno  输出设备序列号  
		e.g 1.`adb get-serialno`
	4. remount \[-R\]  将分区重新挂载为读写模式，如果需要重新启动，则-R选项将自动重新启动设备  
		e.g 1.`adb remount`  
		e.g 2.`adb remount -R`
	5. reboot \[bootloader|recovery|sideload|sideload-auto-reboot\]  
		e.g 1.`adb reboot bootloader`
		- 重新启动设备，默认情况下启动系统镜像，但支持重启到bootloader和recovery
		- 侧载重新启动进入恢复模式，并自动启动侧载模式；侧载自动重新启动，在侧载后重新启动。
	6. sideload OTAPACKAGE  侧载给定的OTA包  
		e.g 1.`adb sideload /path/to/your/ota/package`
	7. root  重启adbd并带有Root权限  
		e.g 1.`adb root`
	8. unroot  重启adbd并不带有Root权限  
		e.g 1.`adb unroot`
	9. usb  重启adbd并在usb上监听  
		e.g 1.`adb usb`
	10. tcpip PORT   重启adbd，并在'端口'上监听TCP/IP  
		e.g 1.`adb tcpip 896450`
9. 内部调试
	1. start-server  确保有服务在运行  
		e.g 1.`adb start-server`
	2. kill-server  如果服务正在运行，则终止服务  
		e.g 1.`adb kill-server`
	3. reconnect  从主机端踢开连接以强制重新连接  
		e.g 1.`adb reconnect`
	4. reconnect device  从设备端踢开连接以强制重新连接  
		e.g 1.`adb reconnect device`
	5. reconnect offline  重置'离线'/'未授权的设备'以强制重新连接  
		e.g 1.`adb reconect offline`
10. USB
	1. attach  连接一个单独的USB设备
	2. detach  从USB设备分离，以允许其他进程使用
11. 环境变量
	- 实在不懂，算了……
## adb shell命令大全(提高版)
1. pm(package manager)包管理器
	```Package manager (package) commands:
	
	  path [--user USER_ID] PACKAGE
	    Print the path to the .apk of the given PACKAGE.
	
	  dump PACKAGE
	    Print various system state associated with the given PACKAGE.
	
	  has-feature FEATURE_NAME [version]
	    Prints true and returns exit status 0 when system has a FEATURE_NAME,
	    otherwise prints false and returns exit status 1
	
	  list features
	    Prints all features of the system.
	
	  list instrumentation [-f] [TARGET-PACKAGE]
	    Prints all test packages; optionally only those targeting TARGET-PACKAGE
	    Options:
	      -f: dump the name of the .apk file containing the test package
	
	  list libraries
	    Prints all system libraries.
	
	  list packages [-f] [-d] [-e] [-s] [-3] [-i] [-l] [-u] [-U] 
	      [--show-versioncode] [--apex-only] [--uid UID] [--user USER_ID] [FILTER]
	    Prints all packages; optionally only those whose name contains
	    the text in FILTER.  Options are:
	      -f: see their associated file
	      -a: all known packages (but excluding APEXes)
	      -d: filter to only show disabled packages
	      -e: filter to only show enabled packages
	      -s: filter to only show system packages
	      -3: filter to only show third party packages
	      -i: see the installer for the packages
	      -l: ignored (used for compatibility with older releases)
	      -U: also show the package UID
	      -u: also include uninstalled packages
	      --show-versioncode: also show the version code
	      --apex-only: only show APEX packages
	      --uid UID: filter to only show packages with the given UID
	      --user USER_ID: only list packages belonging to the given user
	
	  list permission-groups
	    Prints all known permission groups.
	
	  list permissions [-g] [-f] [-d] [-u] [GROUP]
	    Prints all known permissions; optionally only those in GROUP.  Options are:
	      -g: organize by group
	      -f: print all information
	      -s: short summary
	      -d: only list dangerous permissions
	      -u: list only the permissions users will see
	
	  list staged-sessions [--only-ready] [--only-sessionid] [--only-parent]
	    Prints all staged sessions.
	      --only-ready: show only staged sessions that are ready
	      --only-sessionid: show only sessionId of each session
	      --only-parent: hide all children sessions
	
	  list users
	    Prints all users.
	
	  resolve-activity [--brief] [--components] [--query-flags FLAGS]
	       [--user USER_ID] INTENT
	    Prints the activity that resolves to the given INTENT.
	
	  query-activities [--brief] [--components] [--query-flags FLAGS]
	       [--user USER_ID] INTENT
	    Prints all activities that can handle the given INTENT.
	
	  query-services [--brief] [--components] [--query-flags FLAGS]
	       [--user USER_ID] INTENT
	    Prints all services that can handle the given INTENT.
	
	  query-receivers [--brief] [--components] [--query-flags FLAGS]
	       [--user USER_ID] INTENT
	    Prints all broadcast receivers that can handle the given INTENT.
	
	  install [-rtfdg] [-i PACKAGE] [--user USER_ID|all|current]
	       [-p INHERIT_PACKAGE] [--install-location 0/1/2]
	       [--install-reason 0/1/2/3/4] [--originating-uri URI]
	       [--referrer URI] [--abi ABI_NAME] [--force-sdk]
	       [--preload] [--instant] [--full] [--dont-kill]
	       [--enable-rollback]
	       [--force-uuid internal|UUID] [--pkg PACKAGE] [-S BYTES]
	       [--apex] [--staged-ready-timeout TIMEOUT]
	       [PATH [SPLIT...]|-]
	    Install an application.  Must provide the apk data to install, either as
	    file path(s) or '-' to read from stdin.  Options are:
	      -R: disallow replacement of existing application
	      -t: allow test packages
	      -i: specify package name of installer owning the app
	      -f: install application on internal flash
	      -d: allow version code downgrade (debuggable packages only)
	      -p: partial application install (new split on top of existing pkg)
	      -g: grant all runtime permissions
	      -S: size in bytes of package, required for stdin
	      --user: install under the given user.
	      --dont-kill: installing a new feature split, don't kill running app
	      --restrict-permissions: don't whitelist restricted permissions at install
	      --originating-uri: set URI where app was downloaded from
	      --referrer: set URI that instigated the install of the app
	      --pkg: specify expected package name of app being installed
	      --abi: override the default ABI of the platform
	      --instant: cause the app to be installed as an ephemeral install app
	      --full: cause the app to be installed as a non-ephemeral full app
	      --install-location: force the install location:
	          0=auto, 1=internal only, 2=prefer external
	      --install-reason: indicates why the app is being installed:
	          0=unknown, 1=admin policy, 2=device restore,
	          3=device setup, 4=user request
	      --force-uuid: force install on to disk volume with given UUID
	      --apex: install an .apex file, not an .apk
	      --staged-ready-timeout: By default, staged sessions wait 60000
	          milliseconds for pre-reboot verification to complete when
	          performing staged install. This flag is used to alter the waiting
	          time. You can skip the waiting time by specifying a TIMEOUT of '0'
	
	  install-existing [--user USER_ID|all|current]
	       [--instant] [--full] [--wait] [--restrict-permissions] PACKAGE
	    Installs an existing application for a new user.  Options are:
	      --user: install for the given user.
	      --instant: install as an instant app
	      --full: install as a full app
	      --wait: wait until the package is installed
	      --restrict-permissions: don't whitelist restricted permissions
	
	  install-create [-lrtsfdg] [-i PACKAGE] [--user USER_ID|all|current]
	       [-p INHERIT_PACKAGE] [--install-location 0/1/2]
	       [--install-reason 0/1/2/3/4] [--originating-uri URI]
	       [--referrer URI] [--abi ABI_NAME] [--force-sdk]
	       [--preload] [--instant] [--full] [--dont-kill]
	       [--force-uuid internal|UUID] [--pkg PACKAGE] [--apex] [-S BYTES]
	       [--multi-package] [--staged]
	    Like "install", but starts an install session.  Use "install-write"
	    to push data into the session, and "install-commit" to finish.
	
	  install-write [-S BYTES] SESSION_ID SPLIT_NAME [PATH|-]
	    Write an apk into the given install session.  If the path is '-', data
	    will be read from stdin.  Options are:
	      -S: size in bytes of package, required for stdin
	
	  install-remove SESSION_ID SPLIT...
	    Mark SPLIT(s) as removed in the given install session.
	
	  install-add-session MULTI_PACKAGE_SESSION_ID CHILD_SESSION_IDs
	    Add one or more session IDs to a multi-package session.
	
	  install-commit SESSION_ID
	    Commit the given active install session, installing the app.
	
	  install-abandon SESSION_ID
	    Delete the given active install session.
	
	  set-install-location LOCATION
	    Changes the default install location.  NOTE this is only intended for debugging;
	    using this can cause applications to break and other undersireable behavior.
	    LOCATION is one of:
	    0 [auto]: Let system decide the best location
	    1 [internal]: Install on internal device storage
	    2 [external]: Install on external media
	
	  get-install-location
	    Returns the current install location: 0, 1 or 2 as per set-install-location.
	
	  move-package PACKAGE [internal|UUID]
	
	  move-primary-storage [internal|UUID]
	
	  uninstall [-k] [--user USER_ID] [--versionCode VERSION_CODE]
	       PACKAGE [SPLIT...]
	    Remove the given package name from the system.  May remove an entire app
	    if no SPLIT names specified, otherwise will remove only the splits of the
	    given app.  Options are:
	      -k: keep the data and cache directories around after package removal.
	      --user: remove the app from the given user.
	      --versionCode: only uninstall if the app has the given version code.
	
	  clear [--user USER_ID] [--cache-only] PACKAGE
	    Deletes data associated with a package. Options are:
	    --user: specifies the user for which we need to clear data
	    --cache-only: a flag which tells if we only need to clear cache data
	
	  enable [--user USER_ID] PACKAGE_OR_COMPONENT
	  disable [--user USER_ID] PACKAGE_OR_COMPONENT
	  disable-user [--user USER_ID] PACKAGE_OR_COMPONENT
	  disable-until-used [--user USER_ID] PACKAGE_OR_COMPONENT
	  default-state [--user USER_ID] PACKAGE_OR_COMPONENT
	    These commands change the enabled state of a given package or
	    component (written as "package/class").
	
	  hide [--user USER_ID] PACKAGE_OR_COMPONENT
	  unhide [--user USER_ID] PACKAGE_OR_COMPONENT
	
	  suspend [--user USER_ID] PACKAGE [PACKAGE...]
	    Suspends the specified package(s) (as user).
	
	  unsuspend [--user USER_ID] PACKAGE [PACKAGE...]
	    Unsuspends the specified package(s) (as user).
	
	  set-distracting-restriction [--user USER_ID] [--flag FLAG ...]
	      PACKAGE [PACKAGE...]
	    Sets the specified restriction flags to given package(s) (for user).
	    Flags are:
	      hide-notifications: Hides notifications from this package
	      hide-from-suggestions: Hides this package from suggestions
	        (by the launcher, etc.)
	    Any existing flags are overwritten, which also means that if no flags are
	    specified then all existing flags will be cleared.
	
	  grant [--user USER_ID] PACKAGE PERMISSION
	  revoke [--user USER_ID] PACKAGE PERMISSION
	    These commands either grant or revoke permissions to apps.  The permissions
	    must be declared as used in the app's manifest, be runtime permissions
	    (protection level dangerous), and the app targeting SDK greater than Lollipop MR1.
	
	  set-permission-flags [--user USER_ID] PACKAGE PERMISSION [FLAGS..]
	  clear-permission-flags [--user USER_ID] PACKAGE PERMISSION [FLAGS..]
	    These commands either set or clear permission flags on apps.  The permissions
	    must be declared as used in the app's manifest, be runtime permissions
	    (protection level dangerous), and the app targeting SDK greater than Lollipop MR1.
	    The flags must be one or more of [review-required, revoked-compat, revoke-when-requested, user-fixed, user-set]
	
	  reset-permissions
	    Revert all runtime permissions to their default state.
	
	  set-permission-enforced PERMISSION [true|false]
	
	  get-privapp-permissions TARGET-PACKAGE
	    Prints all privileged permissions for a package.
	
	  get-privapp-deny-permissions TARGET-PACKAGE
	    Prints all privileged permissions that are denied for a package.
	
	  get-oem-permissions TARGET-PACKAGE
	    Prints all OEM permissions for a package.
	
	  trim-caches DESIRED_FREE_SPACE [internal|UUID]
	    Trim cache files to reach the given free space.
	
	  list users
	    Lists the current users.
	
	  create-user [--profileOf USER_ID] [--managed] [--restricted] [--ephemeral]
	      [--guest] [--pre-create-only] [--user-type USER_TYPE] USER_NAME
	    Create a new user with the given USER_NAME, printing the new user identifier
	    of the user.
	    USER_TYPE is the name of a user type, e.g. android.os.usertype.profile.MANAGED.
	      If not specified, the default user type is android.os.usertype.full.SECONDARY.
	      --managed is shorthand for '--user-type android.os.usertype.profile.MANAGED'.
	      --restricted is shorthand for '--user-type android.os.usertype.full.RESTRICTED'.
	      --guest is shorthand for '--user-type android.os.usertype.full.GUEST'.
	
	  remove-user [--set-ephemeral-if-in-use | --wait] USER_ID
	    Remove the user with the given USER_IDENTIFIER, deleting all data
	    associated with that user.
	      --set-ephemeral-if-in-use: If the user is currently running and
	        therefore cannot be removed immediately, mark the user as ephemeral
	        so that it will be automatically removed when possible (after user
	        switch or reboot)
	      --wait: Wait until user is removed. Ignored if set-ephemeral-if-in-use
	
	  set-user-restriction [--user USER_ID] RESTRICTION VALUE
	
	  get-max-users
	
	  get-max-running-users
	
	  compile [-m MODE | -r REASON] [-f] [-c] [--split SPLIT_NAME]
	          [--reset] [--check-prof (true | false)] (-a | TARGET-PACKAGE)
	    Trigger compilation of TARGET-PACKAGE or all packages if "-a".  Options are:
	      -a: compile all packages
	      -c: clear profile data before compiling
	      -f: force compilation even if not needed
	      -m: select compilation mode
	          MODE is one of the dex2oat compiler filters:
	            assume-verified
	            extract
	            verify
	            quicken
	            space-profile
	            space
	            speed-profile
	            speed
	            everything
	      -r: select compilation reason
	          REASON is one of:
	            first-boot
	            boot-after-ota
	            post-boot
	            install
	            install-fast
	            install-bulk
	            install-bulk-secondary
	            install-bulk-downgraded
	            install-bulk-secondary-downgraded
	            bg-dexopt
	            ab-ota
	            inactive
	            cmdline
	            second-dex
	            start-app
	            cloud-compile
	            shared
	      --reset: restore package to its post-install state
	      --check-prof (true | false): look at profiles when doing dexopt?
	      --secondary-dex: compile app secondary dex files
	      --split SPLIT: compile only the given split name
	      --compile-layouts: compile layout resources for faster inflation
	
	  force-dex-opt PACKAGE
	    Force immediate execution of dex opt for the given PACKAGE.
	
	  delete-dexopt PACKAGE
	    Delete dex optimization results for the given PACKAGE.
	
	  bg-dexopt-job
	    Execute the background optimizations immediately.
	    Note that the command only runs the background optimizer logic. It may
	    overlap with the actual job but the job scheduler will not be able to
	    cancel it. It will also run even if the device is not in the idle
	    maintenance mode.
	  cancel-bg-dexopt-job
	    Cancels currently running background optimizations immediately.
	    This cancels optimizations run from bg-dexopt-job or from JobScjeduler.
	    Note that cancelling currently running bg-dexopt-job command requires
	    running this command from separate adb shell.
	
	  reconcile-secondary-dex-files TARGET-PACKAGE
	    Reconciles the package secondary dex files with the generated oat files.
	
	  dump-profiles [--dump-classes-and-methods] TARGET-PACKAGE
	    Dumps method/class profile files to
	    /data/misc/profman/TARGET-PACKAGE-primary.prof.txt.
	      --dump-classes-and-methods: passed along to the profman binary to
	        switch to the format used by 'profman --create-profile-from'.
	
	  snapshot-profile TARGET-PACKAGE [--code-path path]
	    Take a snapshot of the package profiles to
	    /data/misc/profman/TARGET-PACKAGE[-code-path].prof
	    If TARGET-PACKAGE=android it will take a snapshot of the boot image
	
	  set-home-activity [--user USER_ID] TARGET-COMPONENT
	    Set the default home activity (aka launcher).
	    TARGET-COMPONENT can be a package name (com.package.my) or a full
	    component (com.package.my/component.name). However, only the package name
	    matters: the actual component used will be determined automatically from
	    the package.
	
	  set-installer PACKAGE INSTALLER
	    Set installer package name
	
	  get-instantapp-resolver
	    Return the name of the component that is the current instant app installer.
	
	  set-harmful-app-warning [--user <USER_ID>] <PACKAGE> [<WARNING>]
	    Mark the app as harmful with the given warning message.
	
	  get-harmful-app-warning [--user <USER_ID>] <PACKAGE>
	    Return the harmful app warning message for the given app, if present
	
	  uninstall-system-updates [<PACKAGE>]
	    Removes updates to the given system application and falls back to its
	    /system version. Does nothing if the given package is not a system app.
	    If no package is specified, removes updates to all system applications.
	
	  get-moduleinfo [--all | --installed] [module-name]
	    Displays module info. If module-name is specified only that info is shown
	    By default, without any argument only installed modules are shown.
	      --all: show all module info
	      --installed: show only installed modules
	
	  log-visibility [--enable|--disable] <PACKAGE>
	    Turns on debug logging when visibility is blocked for the given package.
	      --enable: turn on debug logging (default)
	      --disable: turn off debug logging
	
	  set-silent-updates-policy [--allow-unlimited-silent-updates <INSTALLER>]
	                            [--throttle-time <SECONDS>] [--reset]
	    Sets the policies of the silent updates.
	      --allow-unlimited-silent-updates: allows unlimited silent updated
	        installation requests from the installer without the throttle time.
	      --throttle-time: update the silent updates throttle time in seconds.
	      --reset: restore the installer and throttle time to the default, and
	        clear tracks of silent updates in the system.
	
	  get-app-links [--user <USER_ID>] [<PACKAGE>]
	    Prints the domain verification state for the given package, or for all
	    packages if none is specified. State codes are defined as follows:
	        - none: nothing has been recorded for this domain
	        - verified: the domain has been successfully verified
	        - approved: force approved, usually through shell
	        - denied: force denied, usually through shell
	        - migrated: preserved verification from a legacy response
	        - restored: preserved verification from a user data restore
	        - legacy_failure: rejected by a legacy verifier, unknown reason
	        - system_configured: automatically approved by the device config
	        - >= 1024: Custom error code which is specific to the device verifier
	      --user <USER_ID>: include user selections (includes all domains, not
	        just autoVerify ones)
	  reset-app-links [--user <USER_ID>] [<PACKAGE>]
	    Resets domain verification state for the given package, or for all
	    packages if none is specified.
	      --user <USER_ID>: clear user selection state instead; note this means
	        domain verification state will NOT be cleared
	      <PACKAGE>: the package to reset, or "all" to reset all packages
	  verify-app-links [--re-verify] [<PACKAGE>]
	    Broadcasts a verification request for the given package, or for all
	    packages if none is specified. Only sends if the package has previously
	    not recorded a response.
	      --re-verify: send even if the package has recorded a response
	  set-app-links [--package <PACKAGE>] <STATE> <DOMAINS>...
	    Manually set the state of a domain for a package. The domain must be
	    declared by the package as autoVerify for this to work. This command
	    will not report a failure for domains that could not be applied.
	      --package <PACKAGE>: the package to set, or "all" to set all packages
	      <STATE>: the code to set the domains to, valid values are:
	        STATE_NO_RESPONSE (0): reset as if no response was ever recorded.
	        STATE_SUCCESS (1): treat domain as successfully verified by domain.
	          verification agent. Note that the domain verification agent can
	          override this.
	        STATE_APPROVED (2): treat domain as always approved, preventing the
	           domain verification agent from changing it.
	        STATE_DENIED (3): treat domain as always denied, preveting the domain
	          verification agent from changing it.
	      <DOMAINS>: space separated list of domains to change, or "all" to
	        change every domain.
	  set-app-links-user-selection --user <USER_ID> [--package <PACKAGE>]
	      <ENABLED> <DOMAINS>...
	    Manually set the state of a host user selection for a package. The domain
	    must be declared by the package for this to work. This command will not
	    report a failure for domains that could not be applied.
	      --user <USER_ID>: the user to change selections for
	      --package <PACKAGE>: the package to set
	      <ENABLED>: whether or not to approve the domain
	      <DOMAINS>: space separated list of domains to change, or "all" to
	        change every domain.
	  set-app-links-allowed --user <USER_ID> [--package <PACKAGE>] <ALLOWED>
	      <ENABLED> <DOMAINS>...
	    Toggle the auto verified link handling setting for a package.
	      --user <USER_ID>: the user to change selections for
	      --package <PACKAGE>: the package to set, or "all" to set all packages
	        packages will be reset if no one package is specified.
	      <ALLOWED>: true to allow the package to open auto verified links, false
	        to disable
	  get-app-link-owners [--user <USER_ID>] [--package <PACKAGE>] [<DOMAINS>]
	    Print the owners for a specific domain for a given user in low to high
	    priority order.
	      --user <USER_ID>: the user to query for
	      --package <PACKAGE>: optionally also print for all web domains declared
	        by a package, or "all" to print all packages
	      --<DOMAINS>: space separated list of domains to query for
	
	
	    [-a <ACTION>] [-d <DATA_URI>] [-t <MIME_TYPE>] [-i <IDENTIFIER>]
	    [-c <CATEGORY> [-c <CATEGORY>] ...]
	    [-n <COMPONENT_NAME>]
	    [-e|--es <EXTRA_KEY> <EXTRA_STRING_VALUE> ...]
	    [--esn <EXTRA_KEY> ...]
	    [--ez <EXTRA_KEY> <EXTRA_BOOLEAN_VALUE> ...]
	    [--ei <EXTRA_KEY> <EXTRA_INT_VALUE> ...]
	    [--el <EXTRA_KEY> <EXTRA_LONG_VALUE> ...]
	    [--ef <EXTRA_KEY> <EXTRA_FLOAT_VALUE> ...]
	    [--ed <EXTRA_KEY> <EXTRA_DOUBLE_VALUE> ...]
	    [--eu <EXTRA_KEY> <EXTRA_URI_VALUE> ...]
	    [--ecn <EXTRA_KEY> <EXTRA_COMPONENT_NAME_VALUE>]
	    [--eia <EXTRA_KEY> <EXTRA_INT_VALUE>[,<EXTRA_INT_VALUE...]]
	        (multiple extras passed as Integer[])
	    [--eial <EXTRA_KEY> <EXTRA_INT_VALUE>[,<EXTRA_INT_VALUE...]]
	        (multiple extras passed as List<Integer>)
	    [--ela <EXTRA_KEY> <EXTRA_LONG_VALUE>[,<EXTRA_LONG_VALUE...]]
	        (multiple extras passed as Long[])
	    [--elal <EXTRA_KEY> <EXTRA_LONG_VALUE>[,<EXTRA_LONG_VALUE...]]
	        (multiple extras passed as List<Long>)
	    [--efa <EXTRA_KEY> <EXTRA_FLOAT_VALUE>[,<EXTRA_FLOAT_VALUE...]]
	        (multiple extras passed as Float[])
	    [--efal <EXTRA_KEY> <EXTRA_FLOAT_VALUE>[,<EXTRA_FLOAT_VALUE...]]
	        (multiple extras passed as List<Float>)
	    [--eda <EXTRA_KEY> <EXTRA_DOUBLE_VALUE>[,<EXTRA_DOUBLE_VALUE...]]
	        (multiple extras passed as Double[])
	    [--edal <EXTRA_KEY> <EXTRA_DOUBLE_VALUE>[,<EXTRA_DOUBLE_VALUE...]]
	        (multiple extras passed as List<Double>)
	    [--esa <EXTRA_KEY> <EXTRA_STRING_VALUE>[,<EXTRA_STRING_VALUE...]]
	        (multiple extras passed as String[]; to embed a comma into a string,
	         escape it using "\,")
	    [--esal <EXTRA_KEY> <EXTRA_STRING_VALUE>[,<EXTRA_STRING_VALUE...]]
	        (multiple extras passed as List<String>; to embed a comma into a string,
	         escape it using "\,")
	    [-f <FLAG>]
	    [--grant-read-uri-permission] [--grant-write-uri-permission]
	    [--grant-persistable-uri-permission] [--grant-prefix-uri-permission]
	    [--debug-log-resolution] [--exclude-stopped-packages]
	    [--include-stopped-packages]
	    [--activity-brought-to-front] [--activity-clear-top]
	    [--activity-clear-when-task-reset] [--activity-exclude-from-recents]
	    [--activity-launched-from-history] [--activity-multiple-task]
	    [--activity-no-animation] [--activity-no-history]
	    [--activity-no-user-action] [--activity-previous-is-top]
	    [--activity-reorder-to-front] [--activity-reset-task-if-needed]
	    [--activity-single-top] [--activity-clear-task]
	    [--activity-task-on-home] [--activity-match-external]
	    [--receiver-registered-only] [--receiver-replace-pending]
	    [--receiver-foreground] [--receiver-no-abort]
	    [--receiver-include-background]
	    [--selector]
	    [<URI> | <PACKAGE> | <COMPONENT>]
	```
	1. path \[--user '用户ID'\] '包名'   输出给定包的.apk的路径  
		e.g 1.`adb shell pm path com.android.packagename`  
		e.g 2.`adb shell pm path --user 0 com.android.packagename`  
	2. dump '包名'  输出与给定包名的各种相关的系统状态   
		e.g 1.`adb shell pm dump com.android.packagename`
	3. has-feature '功能API'  \[version(ABI版本)\]  
		e.g 1.`adb shell pm has-feature android.hardware.camera`（将返回true）  
		e.g 2.`adb shell pm has-feature android.hardware.camera 29`  
		- 打印与给定设备相关联的各种功能，并在设备具有FEATURE_NAME时返回退出状态0
	4. list features  输出系统的所有所具备API功能  
		e.g 1.`adb shell pm list features`
	5. list instrumentation \[-f(只打印文件名)\] \[TARGET-PACKAGE(指定包名)\]   
		e.g 1.`adb shell pm list instrumentation`  
		e.g 2.`adb shell pm list instrumentation -f`  
		e.g 3.`adb shell pm list instrumentation -f com.android.testpackage`  
		- 打印所有测试包，可选只针对TARGET-PACKAGE  
	6. list libraries  打印所有系统库  
		e.g 1.`adb shell pm list libraries`
	7. list packages  \[-修饰符\] \[--特殊修饰符\]  打印所有包  
		e.g 1.`adb shell pm list packages -3`(只显示第三方应用包)  
		e.g 1.1.`adb shell pm list packages -3 --show-versioncode`  
		e.g 1.2.`adb shell pm list packages -3 --show-versioncode --apex-only`  
		e.g 2.`adb shell pm list packages -d`(只显示已禁用的应用包)
		- 修饰符大全
		- -f: 只看应用包所在的文件夹
		- -a: 所有应用包 (但是除了apex包) 
		- -d: 过滤只显示已禁用的应用程序包
		- -e: 过滤只显示已启用的应用程序包 
		- -s: 过滤只显示系统应用程序包 
		- -3: 过滤只显示第三方应用程序包
		- -i: 查看应用程序包和它们的安装程序
		- -l: 忽略
		- -U: 也显示应用程序包的 UID 
		- -u: 包括已卸载的应用程序包 
		- --show-versioncode: 同时显示版本代码
		- --apex-only: 只显示APEX包 
		- --uid UID: 过滤只显示具有指定 UID 的应用程序包 
		- --user USER_ID: 仅列出属于指定用户的应用程序包
	9. list permission-groups  打印所有已知的权限组
		e.g 1.`adb shell pm list permission-groups`
	10. list permissions \[-修饰符\] \[GROUP\]  打印所有已知的权限，GROUP可用于指定要列出的权限所属的组  
		e.g 1.`adb shell pm list permissions -s`
		- 修饰符大全
		- -g：按组织权限列表
		- -f：打印所有信息
		- -s：打印简短摘要
		- -d：只列出危险权限
		- -u：仅列出用户将看到的权限
	11. list users  打印出所有用户  
		e.g 1.`adb shell pm list users`
	12. install \[-修饰符\] \[--特殊修饰符\]   
		e.g 1.`adb install -R /path/to/apks`  
		e.g 2.`adb install -g /path/to/apks`  
		e.g 3.`adb install --install-reason 3 /path/to/apks`
		- 安装应用程序。必须提供要安装的apk数据
		- 要么作为文件路径，要么作为从stdin读取
			- 修饰符大全
			- -R  不允许覆盖安装
			- -t  允许测试app安装
			- -i  指定应用程序，所属安装程序的包名
			- -f  将应用程序安装在内部闪存中
			- -d  允许应用降级安装(测试版app专属)
			- -p  部分应用程序安装（在现有包的顶部创建新的拆分）
			- -g  给予运行时的所有权限
			- -S  包大小（以字节为单位），从标准输入读取时需要
			- --user  给指定的用户安装
			- --dont-kill  在应用更新时，不杀死应用进程
			- --restrict-permissions  不将应用权限写入白名单(启动时索取)
			- --originating-uri  设置引发应用程序安装的 URI
			- --referrer  
			- --pkg  为安装的应用apk指定一个包名
			- --abi  覆盖默认ABI
			- --instant  使应用程序作为瞬态安装应用程序安装
			- --full  使应用程序作为非瞬态完整应用程序安装
			- --install-location 0/1/2  强制安装位置：0=自动，1=仅内部，2=优先外部
			- --install-reason 0/1/2/3/4 指示为何安装应用程序: 0=不清楚, 1=管理政策, 2=设备恢复, 3=设备创建, 4=用户要求
			- --force-uuid  在指定 UUID 的磁盘卷上强制安装应用程序
			- --apex  安装一个.apex文件，而不是安装.apk文件
			- --staged-ready-timeout  指定在执行分段安装时等待预重启验证完成的超时时间
	13. set-install-location LOCATION   
		e.g 1.`adb shell pm set-install-location 2`（强制在外部安装）  
		e.g 2.`asb shell pm set-install-location 0`（自动选择)
		- 更改默认安装位置，仅用于调试
		- 使用它可能会导致应用程序崩溃和其他不可预测的行为。
			- 可选LOCATION
			- 0 \[自动\]: 让系统抉择最佳的位置
			- 1 \[内部\]: 在内部设备存储上安装
			- 2 \[外部\]: 在外部介质上安装
	14. get-install-location  根据set-install-location返回当前安装位置，0、1或2  
		e.g 1.`adb shell pm get-install-location`
	15. uninstall \[-k\] \[--user USER_ID\] \[--versionCode VERSION_CODE\] '包名' \[SPLIT...\]  
		e.g 1.`adb uninstall -k com.android.apks`    
		e.g 2.`adb uninstall --versionCode 50 con.android.apks`
		- 从系统中删除给定的包名，可能会删除整个应用程序
		- 如果没有指定SPLIT名称，否则将只删除拆分给定的应用程序
			- 可选项列表
			- -k:   在删除包后保留数据和缓存目录。
			- --user: 从指定的用户中删除应用程序
			- --versionCode: 只有当应用程序有给定的版本代码才卸载
	16. clear \[--user USER_ID\] \[--cache-only\] '包名'  删除与软件包关联的数据  
		e.g 1.`adb shell pm clear com.android.apks`    
		e.g 2.`adb shell pm clear --cache-only com.android.apks` 
		- --user: 指定需要为其清除数据的用户
		- --cache-only: 只需要清除缓存数据
	18. enable \[--user USER_ID(用户代号ID)\] '包名'  启用应用  
		e.g 1.`adb shell pm enable com.android.apks`
	19. disable \[--user USER_ID(用户代号ID)\] '包名'  禁用应用  
		e.g 1.`adb shell pm disable com.android.apks`
	20. disable-user \[--user USER_ID\] '包名'  禁用当前用户应用  
		e.g 1.`adb shell pm disable-user com.android.apks`
	21. hide \[--user USER_ID\] '包名'  隐藏应用  
		e.g 1.`adb shell pm hide com.android.apks`
	22. unhide \[--user USER_ID\] '包名'  显示应用  
		e.g 1.`adb shell om unhide com.android.apks`
	23. suspend \[--user USER_ID\] '包名'  暂停应用  
		e.g 1.`adb shell pm suspend com.android.apks`
	24. unsuspend \[--user USER_ID\] '包名' 启用应用  
		e.g 1.`adb shell pm unsuspend com.android.apks`
	25. set-distracting-restriction \[--user USER_ID\] \[--flag FLAG(若不指定，则为取消限制)\] '包名'  将指定的限制标志设置为给定的包  
		e.g 1.`adb shell pm set-distracting-restriction --flag hide-notifications com.android.apks`
		- 限制标识
		- hide-notifications: 将通知限制
		- hide-from-suggestions:  将推荐限制
	26. grant \[--user USER_ID\] '包名' '权限'  授予应用特定权限(必须在应用的清单中声明为已使用，并且是运行时权限)  
		e.g 1.`adb shell pm grant com.android.apks android.permission.POST_NOTIFICATIONS`
	27. revoke \[--user USER_ID\] '包名' '权限'  剥夺应用特定权限(必须在应用的清单中声明为已使用，并且是运行时权限)  
		e.g 1.`adb shell pm revoke com.android.apks android.permission.POST_NOTIFICATIONS`
	28. set-permission-flags/clear-permission-flags(有难度，懂的自然懂，不做过多赘述)  
	29. reset-permissions \['包名'\]  将所有APP运行时权限恢复到默认状态(关闭)  
		e.g 1.`adb shell pm reset-permissions`
	30. set-permission-enforced PERMISSION \[true|false\]  强制将全部应用权限授予/剥夺  
		e.g 1.`adb shell pm set-permission-enforced android.permission.POST_NOTIFICATIONS true`（强制启动通知权限）
	31. get-privapp-permissions '包名'  输出软件包的特殊权限  
		e.g 1.`adb shell pm get-privapp-permissions com.android.apks`
	32. get-privapp-deny-permissions '包名'  输出软件包拒绝的特殊权限  
		e.g 1.`adb shell pm get-privapp-deny-permissions com.android.apks`
	33. get-oem-permissions '包名'  输出软件包的所有OEM权限 
		e.g 1.`adb shell pm get-oem-permissions com.android.apks`
	34. list users  输出现在目前的用户列表  
		e.g 1.`adb shell pm list users`
	35. create-user \[--修饰符\]  使用给定的用户名创建一个新用户，并打印新的用户标识符  
		e.g 1.`adb shell pm create-user
		- 修饰符大全
		- --user-type “USER TYPE”  “USER TYPE”为用户类型的名称，如果不指定，默认用户类型为android.os.usertype.full.SECONDARY
			- 用户类型大全
			- android.os.usertype.full.SYSTEM
			- android.os.usertype.full.SECONDARY
			- android.os.usertype.full.GUEST
			- android.os.usertype.full.DEMO
			- android.os.usertype.full.RESTRICTED
			- android.os.usertype.profile.MANAGED
			- android.os.usertype.system.HEADLESS
		- --managed  是'--user-type android.os.usertype.profile.MANAGED'的简写
		- --restricted  是'--user-type android.os.usertype.full.RESTRICTED'的简写
		- --guest  是'--user-type android.os.usertype.full.GUEST'的简写
	36. remove-user \[--set-ephemeral-if-in-use | --wait\] USER_ID    
		e.g 1.`adb shell pm remove-user --wait 0`  
		e.g 2.`adb shell pm remove-user --set-ephemeral-if-in-use 0`
		- 删除给定USER_IDENTIFIER的用户，并删除与该用户关联的所有数据
		- --set-ephemeral-if-in-use:  如果用户当前正在运行，因此不能立即删除，会将用户标记为暂时的，以便在可能的情况下(用户使用后、切换或重启)自动删除
		- --wait:  等待用户被移除并忽略'set-ephemeral-if-in-use'
	37. set-user-restriction \[--user USER_ID\] '权限API' '1/0'  控制特定用户的某项OEM权限  
		e.g 1.`adb shell pm set-user-restriction --user 0 no_uninstall_apps 1`(禁止用户卸载APP)  
		e.g 2`adb shell pm set-user-restriction --user 0 no_uninstall_apps 0`(恢复用户卸载APP)
		- API大全
			1. ALLOW_PARENT_PROFILE_APP_LINKING 
				1. `allow_parent_profile_app_linking`  允许应用程序处理来自web的链接
			2. DISALLOW_ADD_MANAGED_PROFILE
				- `no_add_managed_profile`  不允许添加配置文件
			3. DISALLOW_ADD_USER
				- `no_add_user`  不允许添加用户
			4. DISALLOW_ADD_WIFI_CONFIG
				- `no_add_wifi_config`  不允许用户添加新的Wi-Fi配置
			5. DISALLOW_ADJUST_VOLUME
				- `no_adjust_volume`  不允许调节全局音量
			6. DISALLOW_AIRPLANE_MODE
				- `no_airplane_mode`  不允许使用飞行模式
			1. DISALLOW_AMBIENT_DISPLAY
				- `no_ambient_display`  不允许用户显示环境
			2. DISALLOW_APPS_CONTROL
				- `no_control_apps`  让用户失去对于已装载APP的控制权(包括但不限于不可卸载、不可清除数据)
			3. DISALLOW_AUTOFILL
				- `no_autofill`  不使用自动填充服务
			4. DISALLOW_BLUETOOTH
				- `no_bluetooth`  禁用蓝牙
			5. DISALLOW_BLUETOOTH_SHARING
				- `no_bluetooth_sharing`  禁止外发蓝牙共享
			6. DISALLOW_CAMERA_TOGGLE
				- `disallow_camera_toggle`  阻止相机切换
			7. DISALLOW_CELLULAR_2G
				- `no_cellular_2g`  禁止用户使用2G
			8. DISALLOW_CHANGE_WIFI_STATE
				- `no_change_wifi_state`  禁止用户切换WIFI'开/关'状态
			9. DISALLOW_CONFIG_BLUETOOTH
				- `no_config_bluetooth`  禁止用户配置蓝牙
			10. DISALLOW_CONFIG_BRIGHTNESS
				- `no_config_brightness`  禁止用户调节亮度
			11. DISALLOW_CONFIG_CELL_BROADCASTS
				- `no_config_cell_broadcasts`  禁止配置小区广播(volite通话配置的)
			12. DISALLOW_CONFIG_CREDENTIALS
				- `no_config_credentials`  禁止配置用户凭据
			13. DISALLOW_CONFIG_DATE_TIME
				- `no_config_date_time`  禁止配置日期、时间和时区
			14. DISALLOW_CONFIG_DEFAULT_APPS
				- `disallow_config_default_apps`  禁止用户设置默认应用程序
			15. DISALLOW_CONFIG_LOCALE
				- `no_config_locale`  禁止用户更改语言
			16. DISALLOW_CONFIG_LOCATION
				- `no_config_location`  禁止用户切换定位
			17. DISALLOW_CONFIG_MOBILE_NETWORKS
				- `no_config_mobile_networks`  禁止用户配置移动网络
			18. DISALLOW_CONFIG_PRIVATE_DNS
				- `disallow_config_private_dns`  禁止用户配置私人DNS
			19. DISALLOW_CONFIG_SCREEN_TIMEOUT
				- `no_config_screen_timeout`  禁止用户更改屏幕关闭超时
			20. DISALLOW_CONFIG_TETHERING
				- `no_config_tethering`  禁止用户配置和使用便携式热点
			21. DISALLOW_CONFIG_VPN
				- `no_config_vpn`  禁止用户配置VPN
			22. DISALLOW_CONFIG_WIFI
				- `no_config_wifi`  禁止用户配置WIFI
			23. DISALLOW_CONTENT_CAPTURE
				- `no_content_capture`  禁止如OCR之类为目的捕获用户屏幕的内容
			24. DISALLOW_CONTENT_SUGGESTIONS
				- `no_content_suggestions`  同上，限制根据屏幕内容所推送的建议
			25. DISALLOW_CREATE_WINDOWS
				- `no_create_windows`  禁止创建除应用程序窗口之外的窗口
			26. DISALLOW_CROSS_PROFILE_COPY_PASTE
				- `no_cross_profile_copy_paste`  禁止将数据粘贴到其他用户或配置文件中来导出剪贴板内容
			27. DISALLOW_DATA_ROAMING
				- `no_data_roaming`  禁止漫游时用户使用蜂窝数据
			28. DISALLOW_DEBUGGING_FEATURES
				- `no_debugging_features`  禁止用户进行开发者调试
			29. DISALLOW_FACTORY_RESET
				- `no_factory_reset`  禁止用户从设置中进行恢复出厂设置
			30. DISALLOW_FUN(真的很废)
				- `no_fun`  禁止用户触发安卓彩蛋
			31. DISALLOW_GRANT_ADMIN
				- `no_grant_admin`  禁止用户被授予管理权限
			32. DISALLOW_INSTALL_APPS
				- `no_install_apps`  禁止安装APP  
			33. DISALLOW_INSTALL_UNKNOWN_SOURCES
				- `no_install_unknown_sources`  禁止启用未知来源安装
			34. DISALLOW_INSTALL_UNKNOWN_SOURCES_GLOBALLY
				- `no_install_unknown_sources_globally`  禁止所有用户启用未知来源安装
			35. DISALLOW_MICROPHONE_TOGGLE
				- `disallow_microphone_toggle`  禁止麦克风切换
			36. DISALLOW_MODIFY_ACCOUNTS
				- `no_modify_accounts`  指定用户禁止添加和删除帐户
			37. DISALLOW_MOUNT_PHYSICAL_MEDIA
				- `no_physical_media`  禁止用户挂载物理外部介质
			38. DISALLOW_NEAR_FIELD_COMMUNICATION_RADIO
				- `no_near_field_communication_radio`  禁止用户启动NFC
			39. DISALLOW_NETWORK_RESET
				- `no_network_reset`  禁止用户重置网络设置
			40. DISALLOW_OUTGOING_BEAM
				- `no_outgoing_beam`  禁止用户通过NFC传递数据
			41. DISALLOW_OUTGOING_CALLS
				- `no_outgoing_calls`  禁止用户呼出，但紧急呼叫仍然是允许的
			42. DISALLOW_PRINTING
				- `no_printing`  禁止用户打印  
			43. DISALLOW_REMOVE_MANAGED_PROFILE
				- `no_remove_managed_profile`  禁止用户自行删除托管配置文件
			44. DISALLOW_REMOVE_USER
				- `no_remove_user`  禁止用户删除用户
			45. DISALLOW_SAFE_BOOT
				- `no_safe_boot`  禁止用户启动到安全模式
			46. DISALLOW_SET_USER_ICON
				- `no_set_user_icon`  禁止用户更改其图标
			47. DISALLOW_SET_WALLPAPER
				- `no_set_wallpaper`  禁止用户设置更改壁纸
			48. DISALLOW_SHARE_INTO_MANAGED_PROFILE
				- Null
			49. DISALLOW_SHARE_LOCATION
				- `no_share_location`  禁止用户打开位置共享
			50. DISALLOW_SHARING_ADMIN_CONFIGURED_WIFI
				- `no_sharing_admin_configured_wifi`  禁止用户在管理员配置的网络中共享Wi-Fi
			51. DISALLOW_SMS
				- `no_sms`  禁止用户发送或接收短信
			52. DISALLOW_SYSTEM_ERROR_DIALOGS
				- `no_system_error_dialogs`  禁止显示崩溃或无响应应用程序的系统错误对话框,系统自动退出
			53. DISALLOW_ULTRA_WIDEBAND_RADIO
				- `no_ultra_wideband_radio`  禁止使用UWB超宽带
			54. DISALLOW_UNIFIED_PASSWORD
				- `no_unified_password`  禁止从(主从关系)用户使用与主用户相同的锁屏密钥
			55. DISALLOW_UNINSTALL_APPS
				- `no_uninstall_apps`  不允许用户卸载APP
			56. DISALLOW_UNMUTE_MICROPHONE
				- `no_unmute_microphone`  禁止用户调整麦克风音量，并将音量设置为0
			57. DISALLOW_USB_FILE_TRANSFER
				- `no_usb_file_transfer`  禁止通过USB传输文件
			58. DISALLOW_USER_SWITCH
				- `no_user_switch`  禁止用户进行切换用户
			59. DISALLOW_WIFI_DIRECT
				- `no_wifi_direct`  禁止用户使用Wi-Fi Direct
			60. DISALLOW_WIFI_TETHERING
				- `no_wifi_tethering`  禁止使用WI-FI捆绑
			61. ENSURE_VERIFY_APPS
				- `ensure_verify_apps`  禁止用户禁用应用程序验证(双重否定表肯定)
			62. KEY_RESTRICTIONS_PENDING
				- NUll
	38. get-max-users  获取设备上最大支持的用户数量  
		e.g 1.`adb shell pm get-max-users`
	39. get-max-running-users  获取设备上支持的最大同时运行用户数  
		e.g 1.`adb shell pm get-max-users`
	40. compile \[-m '模式' | -r '原因'\] \[-f\] \[-c\] \[--split SPLIT_NAME\]\[--reset\] \[--check-prof (true | false)\] \(-a(全局) | '包名'\)  
			e.g 1.`adb shell pm compile -m everything -f com.android.apks`(完全优化强制编译一个APP)
		- 修饰符大全
		1. -a:  编译所有应用程序包
		2. -c:  编译之前清除配置文件数据
		4. -f:  即使不需要，也进行强制编译
		5. -m '模式':  选择编译模式，可用的dex2oat编译器如下：
			- assume-verified  越过验证
	        - extract  从.dex提取数据
	        - verify  验证.dex字节码
	        - quicken  快速编译模式
	        - space-profile  空间性能折中
	        - space  优化空间利用率
	        - speed-profile  速度性能折中
	        - speed  优化性能
	        - everything  完全优化
	    5. -r '编译原因'：选择编译原因。可用的原因包括：
			- first-boot  首次激活设备后的编译
	        - boot-after-ota  OTA后的编译
	        - post-boot  设备启动后的一般情况下编译
	        - install  安装新应用后的编译
	        - install-fast  快速安装应用后的编译
	        - install-bulk  批量安装应用程序时进行的编译
	        - install-bulk-secondary  批量安装次要应用程序时进行的编译
	        - install-bulk-downgraded  批量降级安装应用程序时进行的编译
	        - install-bulk-secondary-downgraded  批量降级安装次要应用程序时进行的编译
	        - bg-dexopt  后台 dex 优化时进行的编译
	        - ab-ota  AB（A/B）系统 OTA 更新时进行的编译
	        - inactive  非活动应用程序的编译
	        - cmdline  在命令行中指定进行的编译
	        - second-dex  第二个.dex.文件的编译
	        - start-app  启动应用时的编译
	        - cloud-compile  云端编译时的编译
	        - shared  共享库的编译
	    6. --reset：将指定的应用程序包恢复到安装后的状态
	    7. --check-prof (true | false)：在执行 dex2oat 过程时是否查看应用程序的配置文件（profiles,如果设置为 true,则 dex2oat 过程会考虑应用程序的配置文件,以进行更有效的优化;如果设置为false,则不会考虑配置文件
	    8. --secondary-dex：编译应用程序的次要 dex 文件。在一般情况下，应用程序可能包含多个 dex 文件，这个选项可以让 dex2oat 编译程序中所有的 dex 文件。
	    9. --split SPLIT：仅编译指定的拆分名称
	    10. --compile-layouts：编译布局资源，以加速布局膨胀（inflation）
	42. force-dex-opt '包名'  强制执行给定包名的dex-opt操作  
		e.g 1.`adb shell pm force-dex-opt com.android.apks`
	43. delete-dexopt '包名'  删除给定包名的dexopt优化  
		e.g 1.`adb shell pm delete-dexopt com.android.apks`
	44. bg-dexopt-job  在后台给全局APP进行dexopt优化  
		e.g 1.`adb shell pm bg-dexopt-job`
	45. cancel-bg-dexopt-job  取消在后台给全局APP进行dexopt优化  
		e.g 1.`adb shell pm cancel-bg-dexopt-job` 
	46. reconcile-secondary-dex-files '包名'  重新编译使oat文件协调  
		e.g 1.`adb shell pm reconcile-secondary-dex-files com.android.apks`
	47. dump-profiles \[--dump-classes-and-methods\] '包名'  指定包名的方法和类的性能配置文件储存到设备上的`/data/misc/profman/TARGET-PACKAGE-primary.prof.txt`  
		e.g 1.`adb shell pm dump-profiles --dump-classes-and-methods com.android.apks`
	48. snapshot-profile '包名' \[--code-path '路径'\]  对指定应用程序包的性能配置文件进行快照  
		e.g 1.`adb shell pm snapshot-profile com.android.apks`  
		e.g 2.`adb shell pm snapshot-profile com.android.apks --coe-path /path/to/apks/Kernel`
		- 会将目标应用程序包的性能配置文件快照保存到 `/data/misc/profman/TARGET-PACKAGE.prof` 下
		- 修饰符：--code-path path，允许指定目标应用程序的代码路径。如果未指定该选项，默认情况下将使用应用程序的默认代码路径。
		- 如果指定的目标应用程序包是 "android"，则命令将对引导镜像进行快照
	49. set-home-activity \[--user '用户ID'\] '包名'  设置用户的默认启动器  
		e.g 1.`adb shell pm set-home-activity --user 0 com.android.home`
	50. set-installer '包名' '安装器包名' 使用第三方软件包安装器安装软件  
		e.g 1.`adb shell pm set-installer com.android.apks com.android.installer`
	51. get-instantapp-resolver  返回现有的软件包安装器  
		e.g 1.`adb shell pm get-instantapp-resolver`
	52. set-harmful-app-warning \[--user '用户ID'\] '包名' '警告信息'  将应用程序标记为有害，并给出警告信息  
		e.g 1.`adb shell pm set-harmful-app-warning com.android.apks this_is_a_test_log`
	53. get-harmful-app-warning \[--user '用户ID'\] '包名'  返回特定APP的警告信息  
		e.g 1.`adb shell pm get-harmful-app-warning com.android.apks`
	54. uninstall-system-updates \['包名'\]  卸载系统级APP的更新内容，若不指定则全部卸载更新  
		e.g 1.`adb shell pm uninstall-system-updates con.android.apks`
	55. get-moduleinfo \[--all | --installed\] \['模块名称'\]  显示模块的信息  
		e.g 1.`adb shell pm get-moduleinfo --all`(获取全部模块信息)  
		e.g 2.`adb shell pm get-moduleinfo --installed`(获取已安装模块信息)
	56. log-visibility \[--enable|--disable\] '包名'  启用调试日志记录以便当APP的可见性被阻止时生成日志记录  
		e.g 1.`adb shell pm --enable log-visibility com.android.apks`
	57. set-silent-updates-policy \[--allow-unlimited-silent-updates(允许无限静默安装) '包名'\]\[--throttle-time '秒数'\] \[--reset(复原)\]  
		e.g 1.`adb shell pm set-silent-updates-policy --allow-unlimited-silent-updates com.android.packageinstaller`(允许安装器无限静默安装)  
		e.g 2.`adb shell pm set-silent-updates-policy --throttle-time 1`(允许间隔一秒进行静默安装)  
	58. 剩下的是关于软件Debug的，程序员懂的都懂，无需过多赘述
2. am(activity manager)活动管理器（又臭又长）
	```Activity manager (activity) commands:
	
	  help
	
	      Print this help text.
	
	  start-activity [-D] [-N] [-W] [-P <FILE>] [--start-profiler <FILE>]
	
	          [--sampling INTERVAL] [--streaming] [-R COUNT] [-S]
	
	          [--track-allocation] [--user <USER_ID> | current] <INTENT>
	
	      Start an Activity.  Options are:
	
	      -D: enable debugging
	
	      -N: enable native debugging
	
	      -W: wait for launch to complete
	
	      --start-profiler <FILE>: start profiler and send results to <FILE>
	
	      --sampling INTERVAL: use sample profiling with INTERVAL microseconds
	
	          between samples (use with --start-profiler)
	
	      --streaming: stream the profiling output to the specified file
	
	          (use with --start-profiler)
	
	      -P <FILE>: like above, but profiling stops when app goes idle
	
	      --attach-agent <agent>: attach the given agent before binding
	
	      --attach-agent-bind <agent>: attach the given agent during binding
	
	      -R: repeat the activity launch <COUNT> times.  Prior to each repeat,
	
	          the top activity will be finished.
	
	      -S: force stop the target app before starting the activity
	
	      --track-allocation: enable tracking of object allocations
	
	      --user <USER_ID> | current: Specify which user to run as; if not
	
	          specified then run as the current user.
	
	      --windowingMode <WINDOWING_MODE>: The windowing mode to launch the activity into.
	
	      --activityType <ACTIVITY_TYPE>: The activity type to launch the activity as.
	
	      --display <DISPLAY_ID>: The display to launch the activity into.
	
	      --splashscreen-icon: Show the splash screen icon on launch.
	
	  start-service [--user <USER_ID> | current] <INTENT>
	
	      Start a Service.  Options are:
	
	      --user <USER_ID> | current: Specify which user to run as; if not
	
	          specified then run as the current user.
	
	  start-foreground-service [--user <USER_ID> | current] <INTENT>
	
	      Start a foreground Service.  Options are:
	
	      --user <USER_ID> | current: Specify which user to run as; if not
	
	          specified then run as the current user.
	
	  stop-service [--user <USER_ID> | current] <INTENT>
	
	      Stop a Service.  Options are:
	
	      --user <USER_ID> | current: Specify which user to run as; if not
	
	          specified then run as the current user.
	
	  broadcast [--user <USER_ID> | all | current]
	
	          [--receiver-permission <PERMISSION>]
	
	          [--allow-background-activity-starts]
	
	          [--async] <INTENT>
	
	      Send a broadcast Intent.  Options are:
	
	      --user <USER_ID> | all | current: Specify which user to send to; if not
	
	          specified then send to all users.
	
	      --receiver-permission <PERMISSION>: Require receiver to hold permission.
	
	      --allow-background-activity-starts: The receiver may start activities
	
	          even if in the background.
	
	      --async: Send without waiting for the completion of the receiver.
	
	  compact <process_name> <Package UID> [some|full]
	
	      Force process compaction.
	
	      some: execute file compaction.
	
	      full: execute anon + file compaction.
	
	  instrument [-r] [-e <NAME> <VALUE>] [-p <FILE>] [-w]
	
	          [--user <USER_ID> | current]
	
	          [--no-hidden-api-checks [--no-test-api-access]]
	
	          [--no-isolated-storage]
	
	          [--no-window-animation] [--abi <ABI>] <COMPONENT>
	
	      Start an Instrumentation.  Typically this target <COMPONENT> is in the
	
	      form <TEST_PACKAGE>/<RUNNER_CLASS> or only <TEST_PACKAGE> if there
	
	      is only one instrumentation.  Options are:
	
	      -r: print raw results (otherwise decode REPORT_KEY_STREAMRESULT).  Use with
	
	          [-e perf true] to generate raw output for performance measurements.
	
	      -e <NAME> <VALUE>: set argument <NAME> to <VALUE>.  For test runners a
	
	          common form is [-e <testrunner_flag> <value>[,<value>...]].
	
	      -p <FILE>: write profiling data to <FILE>
	
	      -m: Write output as protobuf to stdout (machine readable)
	
	      -f <Optional PATH/TO/FILE>: Write output as protobuf to a file (machine
	
	          readable). If path is not specified, default directory and file name will
	
	          be used: /sdcard/instrument-logs/log-yyyyMMdd-hhmmss-SSS.instrumentation_data_proto
	
	      -w: wait for instrumentation to finish before returning.  Required for
	
	          test runners.
	
	      --user <USER_ID> | current: Specify user instrumentation runs in;
	
	          current user if not specified.
	
	      --no-hidden-api-checks: disable restrictions on use of hidden API.
	
	      --no-test-api-access: do not allow access to test APIs, if hidden
	
	          API checks are enabled.
	
	      --no-isolated-storage: don't use isolated storage sandbox and 
	
	          mount full external storage
	
	      --no-window-animation: turn off window animations while running.
	
	      --abi <ABI>: Launch the instrumented process with the selected ABI.
	
	          This assumes that the process supports the selected ABI.
	
	  trace-ipc [start|stop] [--dump-file <FILE>]
	
	      Trace IPC transactions.
	
	      start: start tracing IPC transactions.
	
	      stop: stop tracing IPC transactions and dump the results to file.
	
	      --dump-file <FILE>: Specify the file the trace should be dumped to.
	
	  profile start [--user <USER_ID> current]
	
	          [--sampling INTERVAL | --streaming] <PROCESS> <FILE>
	
	      Start profiler on a process.  The given <PROCESS> argument
	
	        may be either a process name or pid.  Options are:
	
	      --user <USER_ID> | current: When supplying a process name,
	
	          specify user of process to profile; uses current user if not
	
	          specified.
	
	      --sampling INTERVAL: use sample profiling with INTERVAL microseconds
	
	          between samples.
	
	      --streaming: stream the profiling output to the specified file.
	
	  profile stop [--user <USER_ID> current] <PROCESS>
	
	      Stop profiler on a process.  The given <PROCESS> argument
	
	        may be either a process name or pid.  Options are:
	
	      --user <USER_ID> | current: When supplying a process name,
	
	          specify user of process to profile; uses current user if not
	
	          specified.
	
	  dumpheap [--user <USER_ID> current] [-n] [-g] <PROCESS> <FILE>
	
	      Dump the heap of a process.  The given <PROCESS> argument may
	
	        be either a process name or pid.  Options are:
	
	      -n: dump native heap instead of managed heap
	
	      -g: force GC before dumping the heap
	
	      --user <USER_ID> | current: When supplying a process name,
	
	          specify user of process to dump; uses current user if not specified.
	
	  set-debug-app [-w] [--persistent] <PACKAGE>
	
	      Set application <PACKAGE> to debug.  Options are:
	
	      -w: wait for debugger when application starts
	
	      --persistent: retain this value
	
	  clear-debug-app
	
	      Clear the previously set-debug-app.
	
	  set-watch-heap <PROCESS> <MEM-LIMIT>
	
	      Start monitoring pss size of <PROCESS>, if it is at or
	
	      above <HEAP-LIMIT> then a heap dump is collected for the user to report.
	
	  clear-watch-heap
	
	      Clear the previously set-watch-heap.
	
	  clear-exit-info [--user <USER_ID> | all | current] [package]
	
	      Clear the process exit-info for given package
	
	  bug-report [--progress | --telephony]
	
	      Request bug report generation; will launch a notification
	
	        when done to select where it should be delivered. Options are:
	
	     --progress: will launch a notification right away to show its progress.
	
	     --telephony: will dump only telephony sections.
	
	  fgs-notification-rate-limit {enable | disable}
	
	     Enable/disable rate limit on FGS notification deferral policy.
	
	  force-stop [--user <USER_ID> | all | current] <PACKAGE>
	
	      Completely stop the given application package.
	
	  stop-app [--user <USER_ID> | all | current] <PACKAGE>
	
	      Stop an app and all of its services.  Unlike `force-stop` this does
	
	      not cancel the app's scheduled alarms and jobs.
	
	  crash [--user <USER_ID>] <PACKAGE|PID>
	
	      Induce a VM crash in the specified package or process
	
	  kill [--user <USER_ID> | all | current] <PACKAGE>
	
	      Kill all background processes associated with the given application.
	
	  kill-all
	
	      Kill all processes that are safe to kill (cached, etc).
	
	  make-uid-idle [--user <USER_ID> | all | current] <PACKAGE>
	
	      If the given application's uid is in the background and waiting to
	
	      become idle (not allowing background services), do that now.
	
	  monitor [--gdb <port>]
	
	      Start monitoring for crashes or ANRs.
	
	      --gdb: start gdbserv on the given port at crash/ANR
	
	  watch-uids [--oom <uid>]
	
	      Start watching for and reporting uid state changes.
	
	      --oom: specify a uid for which to report detailed change messages.
	
	  hang [--allow-restart]
	
	      Hang the system.
	
	      --allow-restart: allow watchdog to perform normal system restart
	
	  restart
	
	      Restart the user-space system.
	
	  idle-maintenance
	
	      Perform idle maintenance now.
	
	  screen-compat [on|off] <PACKAGE>
	
	      Control screen compatibility mode of <PACKAGE>.
	
	  package-importance <PACKAGE>
	
	      Print current importance of <PACKAGE>.
	
	  to-uri [INTENT]
	
	      Print the given Intent specification as a URI.
	
	  to-intent-uri [INTENT]
	
	      Print the given Intent specification as an intent: URI.
	
	  to-app-uri [INTENT]
	
	      Print the given Intent specification as an android-app: URI.
	
	  switch-user <USER_ID>
	
	      Switch to put USER_ID in the foreground, starting
	
	      execution of that user if it is currently stopped.
	
	  get-current-user
	
	      Returns id of the current foreground user.
	
	  start-user [-w] <USER_ID>
	
	      Start USER_ID in background if it is currently stopped;
	
	      use switch-user if you want to start the user in foreground.
	
	      -w: wait for start-user to complete and the user to be unlocked.
	
	  unlock-user <USER_ID>
	
	      Unlock the given user.  This will only work if the user doesn't
	
	      have an LSKF (PIN/pattern/password).
	
	  stop-user [-w] [-f] <USER_ID>
	
	      Stop execution of USER_ID, not allowing it to run any
	
	      code until a later explicit start or switch to it.
	
	      -w: wait for stop-user to complete.
	
	      -f: force stop even if there are related users that cannot be stopped.
	
	  is-user-stopped <USER_ID>
	
	      Returns whether <USER_ID> has been stopped or not.
	
	  get-started-user-state <USER_ID>
	
	      Gets the current state of the given started user.
	
	  track-associations
	
	      Enable association tracking.
	
	  untrack-associations
	
	      Disable and clear association tracking.
	
	  get-uid-state <UID>
	
	      Gets the process state of an app given its <UID>.
	
	  attach-agent <PROCESS> <FILE>
	
	    Attach an agent to the specified <PROCESS>, which may be either a process name or a PID.
	
	  get-config [--days N] [--device] [--proto] [--display <DISPLAY_ID>]
	
	      Retrieve the configuration and any recent configurations of the device.
	
	      --days: also return last N days of configurations that have been seen.
	
	      --device: also output global device configuration info.
	
	      --proto: return result as a proto; does not include --days info.
	
	      --display: Specify for which display to run the command; if not 
	
	          specified then run for the default display.
	
	  supports-multiwindow
	
	      Returns true if the device supports multiwindow.
	
	  supports-split-screen-multi-window
	
	      Returns true if the device supports split screen multiwindow.
	
	  suppress-resize-config-changes <true|false>
	
	      Suppresses configuration changes due to user resizing an activity/task.
	
	  set-inactive [--user <USER_ID>] <PACKAGE> true|false
	
	      Sets the inactive state of an app.
	
	  get-inactive [--user <USER_ID>] <PACKAGE>
	
	      Returns the inactive state of an app.
	
	  set-standby-bucket [--user <USER_ID>] <PACKAGE> active|working_set|frequent|rare|restricted
	
	      Puts an app in the standby bucket.
	
	  get-standby-bucket [--user <USER_ID>] <PACKAGE>
	
	      Returns the standby bucket of an app.
	
	  send-trim-memory [--user <USER_ID>] <PROCESS>
	
	          [HIDDEN|RUNNING_MODERATE|BACKGROUND|RUNNING_LOW|MODERATE|RUNNING_CRITICAL|COMPLETE]
	
	      Send a memory trim event to a <PROCESS>.  May also supply a raw trim int level.
	
	  display [COMMAND] [...]: sub-commands for operating on displays.
	
	       move-stack <STACK_ID> <DISPLAY_ID>
	
	           Move <STACK_ID> from its current display to <DISPLAY_ID>.
	
	  stack [COMMAND] [...]: sub-commands for operating on activity stacks.
	
	       move-task <TASK_ID> <STACK_ID> [true|false]
	
	           Move <TASK_ID> from its current stack to the top (true) or
	
	           bottom (false) of <STACK_ID>.
	
	       list
	
	           List all of the activity stacks and their sizes.
	
	       info <WINDOWING_MODE> <ACTIVITY_TYPE>
	
	           Display the information about activity stack in <WINDOWING_MODE> and <ACTIVITY_TYPE>.
	
	       remove <STACK_ID>
	
	           Remove stack <STACK_ID>.
	
	  task [COMMAND] [...]: sub-commands for operating on activity tasks.
	
	       lock <TASK_ID>
	
	           Bring <TASK_ID> to the front and don't allow other tasks to run.
	
	       lock stop
	
	           End the current task lock.
	
	       resizeable <TASK_ID> [0|1|2|3]
	
	           Change resizeable mode of <TASK_ID> to one of the following:
	
	           0: unresizeable
	
	           1: crop_windows
	
	           2: resizeable
	
	           3: resizeable_and_pipable
	
	       resize <TASK_ID> <LEFT,TOP,RIGHT,BOTTOM>
	
	           Makes sure <TASK_ID> is in a stack with the specified bounds.
	
	           Forces the task to be resizeable and creates a stack if no existing stack
	
	           has the specified bounds.
	
	  update-appinfo <USER_ID> <PACKAGE_NAME> [<PACKAGE_NAME>...]
	
	      Update the ApplicationInfo objects of the listed packages for <USER_ID>
	
	      without restarting any processes.
	
	  write
	
	      Write all pending state to storage.
	
	  compat [COMMAND] [...]: sub-commands for toggling app-compat changes.
	
	         enable|disable [--no-kill] <CHANGE_ID|CHANGE_NAME> <PACKAGE_NAME>
	
	            Toggles a change either by id or by name for <PACKAGE_NAME>.
	
	            It kills <PACKAGE_NAME> (to allow the toggle to take effect) unless --no-kill is provided.
	
	         reset <CHANGE_ID|CHANGE_NAME> <PACKAGE_NAME>
	
	            Toggles a change either by id or by name for <PACKAGE_NAME>.
	
	            It kills <PACKAGE_NAME> (to allow the toggle to take effect).
	
	         enable-all|disable-all <targetSdkVersion> <PACKAGE_NAME>
	
	            Toggles all changes that are gated by <targetSdkVersion>.
	
	         reset-all [--no-kill] <PACKAGE_NAME>
	
	            Removes all existing overrides for all changes for 
	
	            <PACKAGE_NAME> (back to default behaviour).
	
	            It kills <PACKAGE_NAME> (to allow the toggle to take effect) unless --no-kill is provided.
	
	  memory-factor [command] [...]: sub-commands for overriding memory pressure factor
	
	         set <NORMAL|MODERATE|LOW|CRITICAL>
	
	            Overrides memory pressure factor. May also supply a raw int level
	
	         show
	
	            Shows the existing memory pressure factor
	
	         reset
	
	            Removes existing override for memory pressure factor
	
	  service-restart-backoff <COMMAND> [...]: sub-commands to toggle service restart backoff policy.
	
	         enable|disable <PACKAGE_NAME>
	
	            Toggles the restart backoff policy on/off for <PACKAGE_NAME>.
	
	         show <PACKAGE_NAME>
	
	            Shows the restart backoff policy state for <PACKAGE_NAME>.
	
	  get-isolated-pids <UID>
	
	         Get the PIDs of isolated processes with packages in this <UID>
	
	  set-stop-user-on-switch [true|false]
	
	         Sets whether the current user (and its profiles) should be stopped when switching to a different user.
	
	         Without arguments, it resets to the value defined by platform.
	
	  set-bg-abusive-uids [uid=percentage][,uid=percentage...]
	
	         Force setting the battery usage of the given UID.

	```
	>Activity活动(\<INTENT\>)语法如下: `com.android.apks/com.android.apks.some.activity`  
	>Service服务(\<INTENT\>)语法如下: `com.android.apks/com.android.apks.some.service`

	1. start-activity \[-修饰符\] \[--特殊修饰符\] 'Activity活动'   启动特定Activity活动  
		e.g 1.`adb shell am start-activity -R 500 com.android.apks/com.android.apks.some.activity`(重启500次测压)  
		e.g 2.`adb shell am start-activity --splashscreen-icon com.android.apks/com.android.apks.some.activity`(在启动前显示图标画面)
		- 修饰符大全
		2. -D:  开启debug模式
		3. -N:  启用本地调试
		4. -W: 等待启动完成
		5. --start-profiler '文件':  启动分析器，并将分析结果发送到指定文件
		6. --sampling '间隔时间(微秒)':  使用间隔为x微秒的样本分析(与--start-profiler一起使用)
		7. --streaming：将分析输出流到指定文件(与--start-profiler 一起使用)
		8. -P '文件':  效果(作用)和上面一样，但是分析会在应用空闲时停止
		9. --attach-agent '代理':  在绑定前附加一个给定的代理
		10. --attach-agent-bind '代理':  在绑定期间附加一个给定代理
		11. -R '次数':  重启活动x次，每次重启之前，都会结束之前的活动
		12. -S:  在启动activity前强制停止目标app
		13. --track-allocation:  启用对象分配跟踪
		14. --user '用户ID' | current:  指定为哪个用户运行，如果未指定，则以当前用户身份运行
		15. --windowingMode '窗口类型(数字)':  启动 Activity 到指定的窗口模式
		16. --activityType '活动类型':  依据指定的活动类型启动指定的Activity
		17. --display '显示器ID':  启动Activity到指定的显示器ID
		18. --splashscreen-icon:  在启动时显示启动画面图标
	2. start-service \[--user <USER_ID> | current\] 'Service服务'  启动一个特定的服务组件(调试用的)  
		e.g 1.`adb shell am start-service current com.android.apks/com.android.apks.some.service`
	3. start-foreground-service \[--user '用户ID' | current\] 'Service服务'  该命令用于启动一个前台服务(调试用的)  
		e.g 1.`adb shell am start-foreground-service com.android.apks/com.android.apks.some.service`
	4. stop-service \[--user <USER_ID> | current\] ''  该命令用于关闭一个特定的服务(调试用的)  
		e.g 1.`adb shell am stop-service com.android.apks/com.android.apks.some.service`
	5. broadcast \[--user <USER_ID> | all | current\] \[--修饰符\] '广播API' 该命令用于发送广播  
		e.g 1.`这我真不懂，没法举例`
		- 修饰符大全
		2. --receiver-permission '权限API':  要求接收方持有权限
		3. --allow-background-activity-starts:  允许接收器在后台启动活动
		4. --async:  异步发送广播，不等待接收器处理完毕
	6. compact '进程名称' '应用包ID' \[some(文件压缩)|full(匿名内存、文件压缩)\]命令用于强制对指定进程进行压缩  
		e.g 1.`adb shell am compact name 10086 some`
	7. set-debug-app \[-w(动时等待调试器附加)\] \[--persistent(重启后保留此设置)\] '包名'  将指定的应用程序设置为调试模式  
		e.g 1.`adb shell am set-debug-app --persistent com.android.apks`  
	8. clear-debug-app  清除先前的'set-debug-app'  
		e.g 1.`adb shell am clear-debug-app`
	9. set-watch-heap '进程名称/PID' '内存限制(字节)'  在指定进程的堆内存上设置监视器，并在堆内存达到指定的内存限制时触发通知  
		e.g 1.`adb shell am set-watch-heap 10086 1024`
	10. clear-watch-heap  清除上方的'set-watch-heap'  
		e.g 1.`adb shell am clear-watch-heap`
	11. clear-exit-info \[--user '用户ID' | all | current\] \['包名'\]  清除指定包名的进程退出信息  
		e.g 1.`adb shell am clear-exit-info all com.android.apks`
	12. bug-report \[--progress(在通知上显示进度) | --telephony(只储存电话相关内容)\]  请求生成 Bug 报告，生成完成后会在通知栏中提醒用户选择报告的传送方式  
		e.g 1.`adb shell am bug-report --progress`
	13. fgs-notification-rate-limit {enable | disable}  用于启用或禁用后台服务通知推迟策略的通知速率限制  
		e.g 1.`adb shell am fgs-notification-rate-limit enable`
	14. force-stop \[--user '用户ID' | all | current\] '包名'  完全停止给定的应用包名  
		e.g 1.`adb shell am force-stop all com.android.apks`
	15. stop-app \[--user '用户ID' | all | current\] '包名'  停止指定包名及其所有服务，但不停止定时提醒和作业  
		e.g 1.`adb shell am stop-app all com.android.apks`
	16. crash \[--user '用户ID'\] '包名/PID'  使特定的PID/包名 程序崩溃  
		e.g 1.`adb shell am crash --user 0 com.android.apks`
	17. kill \[--user '用户ID' | all | current\] '包名' 终止与给定应用程序相关的所有后台进程  
		e.g 1.`adb shell am kill all com.android.apks`
	18. kill-all  杀死所有可以安全杀死的进程(缓存等)  
		e.g 1.`adb shell am kill-all com.android.apks`
	19. make-uid-idle \[--user '用户iD' | all | current\] '包名'  不允许指定包名在后台活动  
		e.g 1.`adb shell am make-uid-idle com.android.apks`
	20. monitor \[--gdb '端口号'\]  开始监视应用程序的崩溃或ANR，并在崩溃时启动GDB服务器  
		e.g 1.`adb shell am monitor --gdb 54321`
	21. watch-uids \[--oom 'UID'\]  开始监视并报告UID状态的变化,并在UID变化时报告  
		e.g 1.`adb shell am watch-uids`
	22. hang \[--allow-restart(允许看门狗正常重启)\]  让系统停止响应  
		e.g 1.`adb shell am hang`
	23. restart  重启用户空间系统  
		e.g 1.`adb shell am restart`
	24. idle-maintenance  立即执行系统的空间维护任务  
		e.g 1.`adb shell am idle-maintenance`
	25. screen-compat \[on|off\] '包名'  手动控制特定应用程序的屏幕兼容模式  
		e.g  1.`adb shell am screen-compat on com.android.apks`
	26. package-importance '包名'  输出指定包名的重要性级别  
		e.g 1.`adb shell am package-importance`
	27. switch-user '用户ID'  切换到将用户ID放在台前，如果该用户当前处于休眠状态，则开始执行  
		e.g 1.`adb shell am switch-user 0`
	28. get-current-user '用户ID'  返回现在的用户  
		e.g 1.`adb shell am get-current-user`
	29. start-user \[-w(等待用户启动完成并解锁)\] '用户ID'  用于启动指定的用户，若要前台启动用户，请使用switch-user  
		e.g 1.`adb shell am start-user -w 0` 
	30. unlock-user '用户ID'  解锁用户(真·锁屏)  
		e.g 1.`adb shell am unlock-user`
	31. stop-user \[-w(等待命令完成)\] \[-f(忽略报错)\] '用户ID'  停止用户的操作，直到显式启动或切换到它  
		e.g 1.`adb shell am stop-user -f 0`
	32. is-user-stopped '用户ID'  返回用户是否关闭状态 
		e.g 1.`adb shell am is-user-stopped`
	33. get-started-user-state '用户ID'  返回用户目前状态  
		e.g 1.`adb shell am get-started-user-state 0` 
	34. track-associations  启用关联追踪  
		e.g 1.`adb shell am track-associations`
	35. untrack-associations 关闭关联追踪  
		e.g 1.`adb shell am untrack-associations`
	36. get-uid-state 'UID'  获取UID进程的工作状态  
		e.g 1.`adb shell am get-uid-state 10086`
	37. get-config  \[--'修饰符'\]  检索设备的配置信息以及最近的配置信息  
		e.g 1.`adb shell am get-config --device`(获取全局信息)
		- 修饰符大全
		2. --days N：同时返回最近x天内出现的配置信息
		3. --device：同时输出全局设备配置信息
		4. --proto：将结果返回为 proto 格式,不包括--days信息
		5. --display：指定要运行命令的显示器,如果未指定，则默认使用默认显示器
	38. supports-multiwindow  返回是否支持多窗口(分屏)  
		e.g 1.`adb shell am supports-multiwindow`
	39. supports-split-screen-multi-window  返回是否支持多窗口(分屏)  
		e.g 1.`adb shell am supports-split-screen-multi-window`
	40. suppress-resize-config-changes true/false  控制是否抑制大小配置更改  
		e.g 1.`adb shell am suppress-resize-config-changes true`
	41. set-inactive \[--user '用户ID'\] '包名' true/false  设置应用是否非活跃状态  
		e.g 1.`adb shell am set-inactive --user 0 com.android.apks true`
	42. get-inactive \[--user '用户ID'\] '包名'  返回应用是否非活跃状态  
		e.g 1.`adb shell am get-inactive --user 0 com.android.apks`
	43. 剩下几个为调试内存相关的，懂得都懂，不过多赘述  
	44. display \[COMMAND\] \[...\] 资料残缺，无法描述，掠过  
	45. update-appinfo '用户ID' '包名1' \['包名N'...\]  用于更新指定用户ID下的一个或多个应用程序的ApplicationInfo对象(简称没啥用)  
		e.g 1.`adb shell am update-appinfo 0 com.android.apk1 com.android.apk2`
	46. write  将所有待处理的状态写入存储  
		e.g 1.`adb shell am write`
	47. memory-factor \[set\[NORMAL/MODERATE/LOW/CRITICAL\]\] \[show\] \[reset\]  
		e.g 1.`adb shell am memory-factor set CRITICAL`(将压力因素设置为临界)
		e.g 2.`adb shell am memory-factor reset`(将压力因素复位)
		- 参数大全
		2. set\[NORMAL/MODERATE/LOW/CRITICAL\]:  内存压力因素值
		3. show:  显示现在内存压力因素
		4. reset: 移除内存压力因素，复位
	48. get-isolated-pids 'UID'  获取指定UID下具有隔离进程的包的PID  
		e.g 1.`adb shell get-isolated-pids 896410`
	49. set-stop-user-on-switch \[true/false\]  设置切换到不同用户时是否应该停止当前用户(及其配置文件)，如果没有参数，它将重置为platform定义的值  
		e.g 1.`adb shell am set-stop-user-on-switch true`
	50. set-bg-abusive-uids \[uid=percentage\] \[,uid=percentage...\]  设置特定 UID 的滥用电池百分比  
		e.g 1.`adb shell set-bg-abusive-uids 100668=30`(设置UID为100668的应用在后台运行时对电池的消耗量为30)
		
