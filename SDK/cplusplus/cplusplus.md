
# 京东云 C++ 签名库
## 基本说明
京东云C++签名工具提供了C++语言访问京东云OpenAPI时的请求签名功能。

在开始调用京东云open API之前，需提前在京东云用户中心账户管理下的[AccessKey管理页面](https://uc.jdcloud.com/accesskey/index)申请accesskey和secretKey密钥对（简称AK/SK）。AK/SK信息请妥善保管，如果遗失可能会造成非法用户使用此信息操作您在云上的资源，给你造成数据和财产损失。

本签名工具使用C++11标准，以静态库的方式提供。使用的大致流程是：
- 将依赖的头文件和静态库通过cmake工具引入您的项目
- 将HTTP请求的信息填充到签名工具的 HttpRequest对象中
- 调用签名接口
- 把返回的HttpRequest对象中Header的Authorization、x-jdcloud-date、x-jdcloud-nonce三项及其值放到您的真实请求Header中
- 然后向京东云OpenAPI网关发起调用

## Linux（Ubuntu）
### 安装方法
1)	安装开发依赖库
```
sudo apt-get install g++ cmake libssl-dev uuid-dev
```
2)	从GitHub下载Demo例子，地址为：https://github.com/jdcloud-api/jdcloud-sdk-cpp-signer

### 使用方法
1)	新建项目工程目录
2)	编写cmake文件，参考Demo中的例子，引用头文件
```
include_directories(${PROJECT_SOURCE_DIR}/h)
include_directories(${PROJECT_SOURCE_DIR}/http)
include_directories(${PROJECT_SOURCE_DIR}/util)
include_directories(${PROJECT_SOURCE_DIR}/util/crypto)
include_directories(${PROJECT_SOURCE_DIR}/util/logging)
```
3)	引用静态库
```
link_libraries(${PROJECT_SOURCE_DIR}/libjdcloudsigner.a)
link_libraries(ssl)
link_libraries(crypto)
link_libraries(uuid)
```
4)	参考Demo中的main.cpp，调用签名接口.详细请见"调用方法"小节。
5)	编译链接
```
cmake .
make
```

## macOS
### 安装方法
1)	安装cmake3.5以上版本
```
brew install cmake
```
2)	从GitHub下载Demo例子，地址为：https://github.com/jdcloud-api/jdcloud-sdk-cpp-signer

### 使用方法
1)	新建项目工程目录
2)	编写cmake文件，参考Demo中的例子，引用头文件
```
include_directories(${PROJECT_SOURCE_DIR}/h)
include_directories(${PROJECT_SOURCE_DIR}/http)
include_directories(${PROJECT_SOURCE_DIR}/util)
include_directories(${PROJECT_SOURCE_DIR}/util/crypto)
include_directories(${PROJECT_SOURCE_DIR}/util/logging)
```
3)	引用静态库（其中ssl库为mac系统自带，不用安装）
```
link_libraries(${PROJECT_SOURCE_DIR}/libjdcloudsigner.a)
link_libraries(ssl)
link_libraries(crypto)
```
4)	参考Demo中的main.cpp，调用签名接口。详细请见"调用方法"小节。
5)	编译链接
```
cmake .
make
```
## Windows
### 安装方法
1)	Visual Stdio 2015以上版本，官方地址为：https://visualstudio.microsoft.com/
2)	CMake 3.5以上版本，官方地址为：https://cmake.org/
3)	从GitHub下载Demo例子，地址为：https://github.com/jdcloud-api/jdcloud-sdk-cpp-signer

### 使用方法
1)	新建项目工程目录
2)	编写cmake文件，参考Demo中的例子，引用头文件
```
include_directories(${PROJECT_SOURCE_DIR}/h)
include_directories(${PROJECT_SOURCE_DIR}/http)
include_directories(${PROJECT_SOURCE_DIR}/util)
include_directories(${PROJECT_SOURCE_DIR}/util/crypto)
include_directories(${PROJECT_SOURCE_DIR}/util/logging)
```
3)	引用静态库
```
link_libraries(${PROJECT_SOURCE_DIR}/jdcloudsigner.lib)
link_libraries(${PROJECT_SOURCE_DIR}/libeay32.lib)
```
4)	参考Demo中的main.cpp，调用签名接口。详细请见"调用方法"小节。
5)	编译链接

打开Visual Studio 开发人员命令提示符，执行
```
cmake .
devenv Demo.sln /build
```

使用Visual Studio打开Demo.sln解决方案，编译。

## 调用方法
```C++
// 引用头文件
#include "Credential.h"
#include "JdcloudSigner.h"
#include "http/HttpTypes.h"
#include "http/HttpRequest.h"
#include "util/logging/Logging.h"
#include "util/logging/ConsoleLogSystem.h"

// 配置日志
ConsoleLogSystem* cls = new ConsoleLogSystem(LogLevel::Debug);
shared_ptr<ConsoleLogSystem> log(cls);
InitializeLogging(log);

// 创建HttpRequest对象
HttpRequest request(URI("YOUR URL"), HttpMethod::HTTP_GET);
request.SetHeaderValue(CONTENT_TYPE_HEADER, "application/json");
request.SetHeaderValue(USER_AGENT_HEADER, "JdcloudSdkGo/1.0.2 vm/0.7.4");

// 创建签名对象
Credential credential("YOUR AK", "YOUR SK");
JdcloudSigner signer(credential, "vm", "cn-north-1");

// 调用签名方法
bool result = signer.SignRequest(request);
if(result)
{
    // 把Header中的三项 "Authorization、x-jdcloud-date、x-jdcloud-nonce" 放到真正的请求头中
    // 向京东云网关发起HTTP请求
}
else
{
    return;
}
```

**注意：**

- 京东云并没有提供其他下载方式，请务必使用上述官方下载方式！

- version 的版本号需要使用京东云产品提供的最新版本号。例如：示例中VM所使用的最新版本号可到官方提供的API  [更新历史](../../API/Virtual-Machines/ChangeLog.md)  中查询到。

- 每支云产品都有自己的Client，当调用该产品API时，需使用该产品的Client。例如：使用云主机的VmClient只能调用云主机（Vm）的接口；使用高可用组的AgClient只能调用高可用组（Ag）的接口。


 
