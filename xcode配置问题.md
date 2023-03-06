# xcode 配置问题汇总

### 报错dependency were found, but they required a higher minimum deployment target
![截屏2023-03-04 08.24.09.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6ac4409555dd4c4292d3bd616b25dd38~tplv-k3u1fbpfcp-watermark.image?)
podfile里面修改platform: ios, '13.0'

### flutter在cocoapods里面的依赖配置
![p2.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c07f45f265164604899569ad28b4d05c~tplv-k3u1fbpfcp-watermark.image?)

### 运行模式：
![p1.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3aed1a9b234948d3a5f05ecc30ae8bce~tplv-k3u1fbpfcp-watermark.image?)

### app.framework 找不到
![p3.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/75662530b39940f68102d7df7449362b~tplv-k3u1fbpfcp-watermark.image?)
.ios 文件没有app.framework包，需要添加


### run script脚本里面的文件路径被注销了，导致找不到
![p4.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d795b57655f34fb586ea240e91af3959~tplv-k3u1fbpfcp-watermark.image?)
重新打开脚本


### OC的Pod三方库 调用 Swift的Pod三方库：
1. 在OC库的Pod文件夹下的`.podspec文件`内添加对该Swift库的依赖(例如：`s.dependency 'TestSwift1', ~'0.1.0'`)，
2. 单独调用该Swift库，只需在这个OC文件内导入Swift库头文件即可(例如：`@import RxSwift;`[不要给库文件名添加<>或者""、不要遗漏分号";"] )，
3. 全局调用该Swift库，在该OC库中创建`.h文件`(可以任意命名，但为了保持统一性，命名规则参照桥接文件的命名规则) -> 在OC库的Pod文件夹下的`.podspec文件`中添加对`.h文件`的引用(`s.prefix_header_contents = '#import "该OC库名-Bridging-Header.h"`)  -> 重新pod update工程 -> 该`.h文件`会自动添加到OC库的`Support Files文件夹`下的`.pch文件`中 -> 在`.h文件`内导入Swift库的头文件即可(例如：`@import RxSwift;`[不要给库文件名添加<>或者""、不要遗漏分号";"] )。

### Swift的Pod三方库 调用 OC的Pod三方库：
1. Swift库内混编：在Swift库中进行库内的混编时，创建OC文件后pod update时，OC文件的头文件会被自动添加到该Swift库的`Support Files文件夹`下的`-umbrella.h文件`中 -> 可以直接调用，
2. Swift库内依赖OC库混编：在该Swift库的`Pod文件夹`下的`.podspec文件`内添加依赖OC库(例如：`s.dependency 'AFNetworking, ~'2.3'`) -> 在主工程的`Podfile文件`中对调用的OC库做特殊处理添加(`:modular_headers => true ->` 例如：`pod 'AFNetworking', '2.3', :modular_headers => true`)，

3. 在Swift库内创建.h文件(可以任意命名，但为了保持统一性，命名规则参照桥接文件的命名规则) -> 此处不需要进行头文件的依赖和引用，直接进行pod update 后，该Swift库的`.h文件`会被自动同步添加到该Swift库的`Support Files文件夹`下的-`umbrella.h文件`中 -> 在Swift库内新建的.h文件中添加需要依赖的OC库的头文件(例如：`#import <AFNetworking/AFNetworking.h>`)，

    4. 编译后调用时才会出现自动提示。

### xcode报错：Command Phasescriptexecution failed with a nonzero exit code，下面显示php command not found? 
说明是php环境没有配置，需要配置php

### php指令
查看php版本 php -v
查询可安装的php版本 brew search php
安装指定版本 brew install php@7.4
将第三方仓库加入brew brew tap shivammathur/php
安装PHP brew install shivammathur/php/php@7.4
卸载 brew uninstall php@7.4
重新安装 brew reinstall php@7.4

### homebrew-core is a shallow clone? 
brew update,首先运行git -C /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core fetch --unshallow

### Error: php@7.4 has been disabled because it is a versioned formula? 
1. sudo vim /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core/Formula/php@7.4.rb 
2. 找到disabled，前面加#

### Error: go: undefined method `xxx’ for? 
brew update


### zsh: command not found: php ？ 
1. 添加 PHP 公式 brew tap shivammathur/php 
2. 选择 PHP 版本——本例使用 7.4 brew install shivammathur/php/php@7.4 
3. 链接 PHP 版本 brew link --overwrite --force php@7.4 
4. 重启终端 查看php -v