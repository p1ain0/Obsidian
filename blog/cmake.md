# CMake

## 工程管理

### cmake_minimum_required

该函数规定了此工程使用的cmake最低版本

由于cmake仍在发展，每个版本均会修改一些函数参数，也会添加更多的函数，此函数的作用是为了防止使用版本过低的cmake来配置导致非预期错误。

### project

此函数声明了此项目的名称

由于一个项目中可能包含多个库或者多个可执行文件，在字库/子可执行程序中禁止使用与该函数声明中的相同的名称。

该函数第二个参数为该项目的代码类型。可声明C或CXX或C CXX。对应的代表了纯c工程，纯c++工程与混合工程。

例如：project(xxx C)

### add_library / add_executable

该函数声明了添加一个库或添加一个可执行程序。第一个参数代表了该库/可执行程序的名称。在没有明确声明生成二进制文件名时，也代表了对应生成的二进制文件名。第二个参数代表了要生成的二进制使用的源文件。这里可以使用列表变量，也可以直接添加源文件名称。当然，也可以在后续使用函数 target_source 添加源文件。

当然，还可以添加其他关键字例如：

- SHARED 声明该库仅被作为动态库生成
- STATIC 声明该库仅被作为静态库生成
- OBJECT 声明该target仅生成中间binary文件，以供其他target使用
- INTERFACE 声明该库仅是一个接口而并没有属于自己的binary
- ALIAS 声明该库仅是其他库的别名
- IMPORTED 声明该库不需要构建，而是已被导入具体配置。此方式一般存在于依赖提供的配置中。

上述关键字只能在 add_library 中被声明。在未声明前五个关键字时，库的构建类型根据 BUILD_SHARED_LIBS 变化。
注意：声明win32可执行程序时，应在add_executable中程序名称后添加关键字 WIN32 。

### 关键字 project

如上面所讲，每个cmake项目均应当声明此关键字，这影响了整个项目的属性。cmake也会提供项目对应的各个变量，例如：

- PROJECT_NAME 项目名称
- PROJECT_SOURCE_DIR 项目源码根目录
- PROJECT_VERSION 项目版本
- PROJECT_BINARY_DIR 项目生成的临时二进制目录，用于存放配置/编译中间文件。

### 关键词target

target在cmake中是一个很重要的概念，你应当将它理解为一个object。它包含了例如以下内容：

- 相关的源文件列表
- 相关的编译选项
- 相关的依赖库
- 相关的头文件路径列表
- 相关的库文件路径列表
- 相关的其他属性

所以，你在后续可以使用这个target名称做任何事情。对应的cmake函数会使用该名称自动提取函数需使用的属性值。

### 关键词PUBLIC PRIVATE INTERFACE

这些关键字是用于搭建依赖关系

- PUBLIC 声明该关键字后续的值在构建该target时使用，并向下游提供。
- PRIVATE 声明该关键字后续的值仅在构建该target时使用，不向下游提供。
- INTERFACE 声明该关键字仅向下游提供，不在构建该target时使用。

```cmake
cmake_minimum_required(VERSION 3.0)
project(sample CXX)
add_library(sample sample.cpp)
add_executable(sample_exe sample_exe.cpp)
```

## 参数

命令行参数对于命令中的参数：

- -S顶级CMakeLists.txt（包含project声明）所在路径。
- -B存放临时编译的二进制文件（.obj、.ilk等）和编译器对应的配置文件路径。
- -G编译器名称-A架构名称-D使用该变量以向cmake传入各种参数。包括选项及覆盖cmake提供的各种默认变量值。
- --toolchaincmake toolchain文件路径。这一点将在后续讲解。
- --install-prefix安装的二进制存放路径。
- --trace / --trace-expand调试时使用，用于打印已执行的cmake代码及行号。否则仅输出函数message中的内容。
- --build使用cmake直接调用编译器编译项目。
- --config选择需要编译的项目配置类型。
- --install安装已编译好的二进制文件至 CMAKE_INSTALL_PREFIX 中。

## 依赖管理

### 查找依赖

find_package
find_package旨在使用预先设置的配置文件来查找依赖项，其函数原型为：

```cmake
find_package(PACKAGE_NAME_CASE_SENSITIVE
             [version] [EXACT] [QUIET]
             [REQUIRED] [[COMPONENTS] [components...]]
             [OPTIONAL_COMPONENTS components...]
             [CONFIG|NO_MODULE]
             [NO_POLICY_SCOPE]
             [NAMES name1 [name2 ...]]
             [CONFIGS config1 [config2 ...]]
             [HINTS path1 [path2 ... ]]
             [PATHS path1 [path2 ... ]]
             [PATH_SUFFIXES suffix1 [suffix2 ...]]
             [NO_DEFAULT_PATH]
             [NO_PACKAGE_ROOT_PATH]
             [NO_CMAKE_PATH]
             [NO_CMAKE_ENVIRONMENT_PATH]
             [NO_SYSTEM_ENVIRONMENT_PATH]
             [NO_CMAKE_PACKAGE_REGISTRY]
             [NO_CMAKE_BUILDS_PATH] # Deprecated; does nothing.
             [NO_CMAKE_SYSTEM_PATH]
             [NO_CMAKE_SYSTEM_PACKAGE_REGISTRY]
             [CMAKE_FIND_ROOT_PATH_BOTH |
              ONLY_CMAKE_FIND_ROOT_PATH |
              NO_CMAKE_FIND_ROOT_PATH)
```

- PACKAGE_NAME_CASE_SENSITIVE

这个名称表示了需要查找的库的名称，大小写敏感，与之对应的是调用了包含此名称的配置文件。如果使用该函数始终找不到库，请仔细查看文件名称中包含的库名称大小写。

- version

依赖的版本号。

如果依赖的配置同时提供了版本文件，则会使用该值对比配置中的版本而确定是否可以使用。

- EXACT

该关键字声明了版本号必须严格对应配置中的版本号后，此依赖才可以被使用。大一点小一点均不可。

- QUIET

该关键字关闭了查找信息（不包含查找失败/错误信息）的输出。

- CONFIG

该关键字声明了需要使用 依赖项通过自己的cmake代码 使用cmake 自动生成的 配置文件，入口配置文件名称一般为 <LOW_CASE_PACKAGE_NAME>-config.cmake 或 <ALL_CASE_PACKAGE_NAME>Config.cmake 。

请注意一个是使用全小写的依赖名称而另一个使用包含大小写的依赖项名称。这一点很多人忽略了，导致了自己项目中始终找不到配置文件，即使查找路径正确。

额外使用 version 关键字时，cmake会调用文件 <LOW_CASE_PACKAGE_NAME>-config-version.cmake 或 <ALL_CASE_PACKAGE_NAME>ConfigVersion.cmake 来查看版本是否匹配。

由于是通过依赖项的cmake配置自动生成的，一般情况下不会过期或出现错误。所以我推荐使用此模式。

对于查找路径来说，是我们特别容易忽略又非常重要的一点。

官方文档是：find_package - CMake 3.23.0-rc5 Documentation 。

简单说下需要注意的几点：

    <PACKAGE_NAME>_DIR 你可以在 find_package 之前直接设置这个宏至依赖包路径下来告诉cmake需要查找的路径。对于一些特殊的场景有奇效。
    CMAKE_PREFIX_PATH cmake统一使用该list中的路径来查找所有依赖项。所以如果你把依赖项都放在一起，请将他们所在的根目录 APPEND 或 REPEND 到此list中。
    CMAKE_FRAMEWORK_PATH 此路径为MacOS专用，存放系统 framework 的依赖项位置。
    CMAKE_APPBUNDLE_PATH 此路径为MacOS专用，存放 app bundle的位置。

对于上述前两个变量/列表而言，cmake会在每一条的以下扩展路径中查找配置文件:

    <EACH_PATH>/ 根目录
    <EACH_PATH>/cmake
    <EACH_PATH>/<PACKAGE_NAME>
    <EACH_PATH>/<PACKAGE_NAME>/cmake
    <EACH_PATH>/<lib/share>/cmake
    <EACH_PATH>/<lib/share>/<PACKAGE_NAME>
    <EACH_PATH>/<lib/share>/<PACKAGE_NAME>/cmake
    <EACH_PATH>/<PACKAGE_NAME>/<lib/share>/<PACKAGE_NAME>
    <EACH_PATH>/<PACKAGE_NAME>/<lib/share>/<PACKAGE_NAME>/cmake

很晕是不是？其实通用做法很简单：将你需要提供的配置文件扔到 root/share/<PACKAGE_NAME> 或 root/lib/cmake/<PACKAGE_NAME> 中就行了。

使用该模式时，一般情况下会提供依赖项对应的 target 名称（包含或不包含namespace）以供使用。极少数情况下也提供依赖项对应的各种宏，这主要是为了兼容预先cmake提供的MODULE模式文件使用方式（设置CMAKE_FIND_PACKAGE_PREFER_CONFIG 而无需修改任何 find_package 代码即可优先使用 CONFIG 模式）。

当未使用 REQUIRED 关键字时，我们可以使用 <PACKAGE_NAME>_FOUND 来判断是否找打了合适的依赖项。

该关键字不可与下一关键字同时声明。

- MODULE

该关键字声明了需要使用 cmake官方 或 依赖项本身 或 本项目 提供的 Find<PACKAGE_NAME>.cmake 文件。

使用了上一条相同的查找规则。需要特别注明的是，它提供了一个额外的查找路径 CMAKE_MODULE_PATH ，你可以将自己写好的配置文件路径 PREPEND 到这个 list中来优先使用你的配置文件。

额外使用 version 关键字时，cmake会使用依赖项中某些文件（例如头文件或README）中存在的版本号对应配置文件中的宏 <PACKAGE_NAME>_VERSION 来查看版本是否匹配。

使用该模式时，一般情况下会提供依赖项对应的宏以供使用：

    <PACKAGE_NAME>_INCLUDE_DIRS / <PACKAGE_NAME>_INCLUDE_DIR 头文件路径
    <PACKAGE_NAME>_LIBRARIES / <PACKAGE_NAME>_LIBRARY 库名称（包含路径和配置表达式）
    <PACKAGE_NAME>_VERSION / <PACKAGE_NAME>_VERSION_STRING 完整版本号
    <PACKAGE_NAME>_FOUND 是否查找到该依赖

而在少数情况下也提供 target 名称以供下游使用。

注意：在没有声明上一个及这个关键字时，cmake会根据 CMAKE_FIND_PACKAGE_PREFER_CONFIG 的设置来判断优先使用 CONFIG 模式或 MODULE 模式。

- NAMES

将此关键字后面的名称替换PACKAGE_NAME_CASE_SENSITIVE来查找配置文件，并提供此名称对应的宏。

由于带“S”，你可以多写几个来匹配。

- PATHS

声明查找配置文件的根目录，相当于 CMAKE_PREFIX_PATH， 不过是额外的。

- PATH_SUFFIXES

声明查找根目录下的额外扩展相对路径。

- NO_DEFAULT_PATH

不使用默认路径查找，也就是说不使用上面列举的以 CMAKE_ 为前缀的路径，而必须设置<PAKCAGE_NAME>_DIR 或 PATHS 来查找。

这一项在禁用cmake自带的MODULE文件时有奇效。

- NO_SYSTEM_PATH

不在系统路径中查找。
find_library

这是原始的cmake查找依赖方式：直接查找依赖项库文件，一般与下面的一项同时使用。

其函数原型为：

```
find_library (
          <LIBRARY_NAME>
          name | NAMES name1 [name2 ...] [NAMES_PER_DIR]
          [HINTS [path | ENV var]... ]
          [PATHS [path | ENV var]... ]
          [PATH_SUFFIXES suffix1 [suffix2 ...]]
          [DOC "cache documentation string"]
          [NO_CACHE]
          [REQUIRED]
          [NO_DEFAULT_PATH]
          [NO_PACKAGE_ROOT_PATH]
          [NO_CMAKE_PATH]
          [NO_CMAKE_ENVIRONMENT_PATH]
          [NO_SYSTEM_ENVIRONMENT_PATH]
          [NO_CMAKE_SYSTEM_PATH]
          [CMAKE_FIND_ROOT_PATH_BOTH |
           ONLY_CMAKE_FIND_ROOT_PATH |
           NO_CMAKE_FIND_ROOT_PATH]
         )
```

这与上面的 find_package 略有不同，我仅在此说明不同之处：

    LIBRARY_NAME 由于直接查找库文件而不是查找配置文件，此名称仅作为结果中宏的前缀使用。
    NAMES 此项声明了库文件的名称。值得注意的是，在UNIX-style系统中，自动添加“lib”作为库名称的前缀。
    NAMES_PER_DIR 一个名称遍历查找一次，再用另一个名称遍历查找一次。而不是根据路径使用多个名称遍历。

查找完成后：

    如果查找到，则会设置 LIBRARY_NAME 为查找到的库文件的名称（包含全路径）。
    如果没有查找到，则会将 LIBRARY_NAME 设置为 <LIBRARY_NAME>-NOTFOUND 。

所以这里和 find_package 又有不同，我们应当使用以下代码判断是否查找到:

if (PACKAGE_NAME MATCHES "-NOTFOUND")
    message(FATAL_ERROR "${PACKAGE_NAME} not found!")
endif()

find_path

这个函数一般是查找头文件或其他的 非库文件 且 非可执行程序。其函数原型为：

```
find_path (
          <FILE_NAME>
          name | NAMES name1 [name2 ...]
          [HINTS [path | ENV var]... ]
          [PATHS [path | ENV var]... ]
          [PATH_SUFFIXES suffix1 [suffix2 ...]]
          [DOC "cache documentation string"]
          [NO_CACHE]
          [REQUIRED]
          [NO_DEFAULT_PATH]
          [NO_PACKAGE_ROOT_PATH]
          [NO_CMAKE_PATH]
          [NO_CMAKE_ENVIRONMENT_PATH]
          [NO_SYSTEM_ENVIRONMENT_PATH]
          [NO_CMAKE_SYSTEM_PATH]
          [CMAKE_FIND_ROOT_PATH_BOTH |
           ONLY_CMAKE_FIND_ROOT_PATH |
           NO_CMAKE_FIND_ROOT_PATH]
         )
```

参数没什么好说的，参考上一条。

一般情况下，由于需要cmake表达式来让cmake判断使用哪个配置的库，我们通常这么写：

```
find_path(<PACKAGE_NAME>_INCLUDE_DIR NAMES header.h PATH_SUFFIXES include/...)

find_library(<PACKAGE_NAME>_LIBRARY_RELEASE NAMES name1 name2)
find_library(<PACKAGE_NAME>_LIBRARY_DEBUG NAMES name1d name2d)
select_library_configurations(<PACKAGE_NAME>)
...
target_*(target_name ${<PACKAGE_NAME>})

find_program
```

这个函数专门用于查找可执行程序。其函数原型为：

```
find_program (
          <VAR>
          name | NAMES name1 [name2 ...] [NAMES_PER_DIR]
          [HINTS [path | ENV var]... ]
          [PATHS [path | ENV var]... ]
          [PATH_SUFFIXES suffix1 [suffix2 ...]]
          [DOC "cache documentation string"]
          [NO_CACHE]
          [REQUIRED]
          [NO_DEFAULT_PATH]
          [NO_PACKAGE_ROOT_PATH]
          [NO_CMAKE_PATH]
          [NO_CMAKE_ENVIRONMENT_PATH]
          [NO_SYSTEM_ENVIRONMENT_PATH]
          [NO_CMAKE_SYSTEM_PATH]
          [CMAKE_FIND_ROOT_PATH_BOTH |
           ONLY_CMAKE_FIND_ROOT_PATH |
           NO_CMAKE_FIND_ROOT_PATH]
         )
```
也没什么好说的，直接使用 PROGRAM_NAME 就好了。
使用依赖

经过了上面的狂轰乱炸，我们终于可以使用依赖项了。我们可以将查找到的依赖项用于多个函数中，例如添加头文件路径，添加链接库，添加编译选项等。

对于不同的查找方式，配置文件或cmake提供了不同的使用方式：
宏

例如 `<PACKAGE_NAME>_INCLUDE_DIRS` 和 `<PACKAGE_NAME>_LIBRARIES` 这种方式。

对于头文件来讲，直接加到include_directories中就好了。而对于库来讲，则复杂点：

由于不能混合使用debug库及release库，cmake必须明确知道在不同配置下使用哪个库。所以宏中一般使用到了cmake表达式来处理这种情况：cmake-generator-expressions(7) - CMake 3.23.0-rc5 Documentation

比如：

```
$<$<CONFIG:DEBUG>:library.lib> $<${NOT:$<CONFIG:DEBUG>>:libraryd.lib>
```
所以我们在写配置时，尽量将debug和release库均查找后使用 select_library_configurations 来生成表达式以便不同配置下使用。
target

target 就简单的多了，因为它是一个object，cmake函数可以轻松提取 target 包含的需要使用的属性来使用。

当然，target 包含非namespace与namespace两种形式，不过使用上没区别。
内部依赖

对于大型项目来说，我们可能需要声明不同的target，并将这些target建立依赖关系。所以我们通常使用以下方式：
add_dependencies

函数原型为：

add_dependencies(<target> [<target-dependency>]...)

这个很简单，向前者添加依赖项（后者），可以添加多个。在编译或某些配置时，优先处理后者。