#cmake 


##相关代码
```c
# Set the minimum version of CMake that can be used
# To find the cmake version run
# $ cmake --version
cmake_minimum_required(VERSION 3.5)

# Set the project name
project (hello_cmake)

# Add an executable
add_executable(hello_cmake main.cpp)

```
```javascript

cmake_minimum_required(VERSION 3.20)
​
project(CMK-Hello
        VERSION 0.0.1
        DESCRIPTION "A Test Project"
        LANGUAGES CXX
)
# set language std
set(CMAKE_CXX_STANDARD 17)
# set language std and Disable fallback to a previous version
set(CMAKE_CXX_STANDARD_REQUIRED ON)
# Disable CXX Syntax EXTENSIONS
set(CMAKE_CXX_EXTENSIONS OFF)
​
​
set(STATIC_HEADER "include/static/")
add_library(myLib STATIC src/STD_Suggestion.cpp ${STATIC_HEADER}STD_Suggestion.h)
​
# for #include "static/Suggestion.h"
target_include_directories(myLib
    INTERFACE
        include/
)
​
set(SOURCES
    src/hello.cpp
)
​
add_executable(HelloExe ${SOURCES})
​
target_link_libraries(HelloExe
    PRIVATE
        myLib
)
```
## 相关代码解释

```javascript
cmake_minimum_required(VERSION 3.5)
```
定义使用的cmake的最低版本

```javascript
project(hello_library)
```
定义项目名称

```javascript
add_library(<name> [STATIC | SHARED | MODULE]
            [EXCLUDE_FROM_ALL]
            source1 [source2 ...])

set(STATIC_HEADER "include/static/")

add_library(myLib STATIC src/STD_Suggestion.cpp ${STATIC_HEADER}STD_Suggestion.h)

```
给cmake增加一个名为mylib的库，这行代码会在build/下创建一个文件夹名为mylib.dir。


```javascript
include_directories("/opt/MATLAB/R2012a/extern/include")
link_directories( ${PROJECT_SOURCE_DIR}/lib/linux)
```

第一行代码为设置除默认include目录外的include目录
第二行代码为设置除默认链接目录外的新的链接目录

###链接各种库，important
```javascript
    link_directories( ${PROJECT_SOURCE_DIR}/lib/linux)
    link_libraries(
        lib1.a
        lib2.a
    )
```

```
(1) link_libraries用在add_executable之前，target_link_libraries用在add_executable之后
(2) link_libraries用来链接静态库，target_link_libraries用来链接导入库，即按照header file + .lib + .dll方式隐式调用动态库的.lib库
```

```javascript
add_library(myLib STATIC src/STD_Suggestion.cpp ${STATIC_HEADER}STD_Suggestion.h)
​
target_include_directories(myLib
#   PRIVATE 
    INTERFACE
        include/
)
add_executable(HelloExe ${SOURCES})
​
target_link_libraries(HelloExe
    PRIVATE
        myLib
)
```

target_include_directories:指定mylib编译，链接时要找的对象

target_link_libraries 指定生成的target以及这个target需要链接的库的名称,即定义生成的文件的链接时使用的库。

add_executable 生成最终的可执行文件

```javascript
find_path(Libcurl_INCLUDE_DIR NAMES curl/curl.h PATHS /usr/local/include)
```
在path路径下寻找 curl/curl.h 将其命令为临时变量 Libcurl_INCLUDE_DIR

```javascript
find_library(Libcurl_LIBRARY NAMES  libcurl curl PATHS /usr/local/lib)
```
在path路径下寻找前缀为libcurl或者curl的库，并将其命令为临时变量 Libcurl_LIBRARY






###导入第三方库的一个example

```javascript
cmake_minimum_required(VERSION 3.9)
project(nihao)
# set language std
set(CMAKE_CXX_STANDARD 17)
# set language std and Disable fallback to a previous version
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(Libcurl_DIRS "/usr/local/include")
#set(Libcurl_LIBS "/usr/local/lib/libcurl.a")
include_directories(${Libcurl_DIRS})
link_directories(/usr/local/lib)
add_executable(nihao main.cpp)
target_link_libraries(nihao -lcurl)
```










#libcurl相关


描述：用来进行http操作的api调用。

##example
```cpp
#include <stdio.h>
#include <curl/curl.h>
 
int main(void)
{
  CURL *curl;
  CURLcode res;
 
  curl = curl_easy_init();
  if(curl) {
    curl_easy_setopt(curl, CURLOPT_URL, "https://example.com");
    /* example.com is redirected, so we tell libcurl to follow redirection */
    curl_easy_setopt(curl, CURLOPT_FOLLOWLOCATION, 1L);
 
    /* Perform the request, res will get the return code */
    res = curl_easy_perform(curl);
    /* Check for errors */
    if(res != CURLE_OK)
      fprintf(stderr, "curl_easy_perform() failed: %s\n",
              curl_easy_strerror(res));
 
    /* always cleanup */
    curl_easy_cleanup(curl);
  }
  return 0;
}
```

##curl_easy_setopt部分函数解释


### CURLOPT_WRITEDATA

描述：返回报文写入一个FILE*指针
用法：

```cpp
FILE *fp;
CURL *curl;
curl_easy_setopt(curl, CURLOPT_WRITEDATA, fp);

```
### CURLOPT_HTTPHEAFER
描述：设置请求报文中附加的报文头
用法：
```cpp
    struct curl_slist *headers = nullptr;
    headers = curl_slist_append(headers, "Content-Type: application/json");
    curl_easy_setopt(curl, CURLOPT_HTTPHEADER,headers);
```
###设置请求内容为json格式
用法：
```cpp
    std::string jsonObj = "{\"int64_test\":20}";
    struct curl_slist *headers = nullptr;
    headers = curl_slist_append(headers, "Content-Type: application/json");
    curl_easy_setopt(curl, CURLOPT_HTTPHEADER,headers);
    curl_easy_setopt(curl, CURLOPT_POSTFIELDS,mydata.c_str());
```
###CURLOPT_CUSTOMREQUEST
描述：设置请求报文的请求方式，例如get,post,put,delete等
用法:
```cpp
    curl_easy_setopt(curl,CURL_CUSTOPMREQUEST,c_str());
```
###CURLOPT_VERBOSE
描述：设置请求返回时返回很多额外信息，如报文头等
用法：
```cpp
    curl_easy_setopt(curl,CURL_VERBOSE,1L);
```

#踩到的雷


1.nebula定义的StatusOr<<std::string>>有判断是否为空，验证时需要将返回报文输出到文件夹

2."w"只写！！！，“r"只读！！！写代码小心。

3.变量为声明时，不一定是库没找到，仔细查看是不是变量名字写错了。

4.在使用 CURL_POSTFIELDS的时候，里面的 c_str()是有可能改变的，随时注意变量的情况。

5.FINDxxxx.cmake函数如何声明。

6.cmake需要时刻用到才会精通，注意平时对cmake的及时复习。

#后记

最近需要学习gdb调试









