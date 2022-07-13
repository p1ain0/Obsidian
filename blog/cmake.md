# CMake

## ���̹���

### cmake_minimum_required

�ú����涨�˴˹���ʹ�õ�cmake��Ͱ汾

����cmake���ڷ�չ��ÿ���汾�����޸�һЩ����������Ҳ����Ӹ���ĺ������˺�����������Ϊ�˷�ֹʹ�ð汾���͵�cmake�����õ��·�Ԥ�ڴ���

### project

�˺��������˴���Ŀ������

����һ����Ŀ�п��ܰ����������߶����ִ���ļ������ֿ�/�ӿ�ִ�г����н�ֹʹ����ú��������е���ͬ�����ơ�

�ú����ڶ�������Ϊ����Ŀ�Ĵ������͡�������C��CXX��C CXX����Ӧ�Ĵ����˴�c���̣���c++�������Ϲ��̡�

���磺project(xxx C)

### add_library / add_executable

�ú������������һ��������һ����ִ�г��򡣵�һ�����������˸ÿ�/��ִ�г�������ơ���û����ȷ�������ɶ������ļ���ʱ��Ҳ�����˶�Ӧ���ɵĶ������ļ������ڶ�������������Ҫ���ɵĶ�����ʹ�õ�Դ�ļ����������ʹ���б������Ҳ����ֱ�����Դ�ļ����ơ���Ȼ��Ҳ�����ں���ʹ�ú��� target_source ���Դ�ļ���

��Ȼ����������������ؼ������磺

- SHARED �����ÿ������Ϊ��̬������
- STATIC �����ÿ������Ϊ��̬������
- OBJECT ������target�������м�binary�ļ����Թ�����targetʹ��
- INTERFACE �����ÿ����һ���ӿڶ���û�������Լ���binary
- ALIAS �����ÿ����������ı���
- IMPORTED �����ÿⲻ��Ҫ�����������ѱ�����������á��˷�ʽһ������������ṩ�������С�

�����ؼ���ֻ���� add_library �б���������δ����ǰ����ؼ���ʱ����Ĺ������͸��� BUILD_SHARED_LIBS �仯��
ע�⣺����win32��ִ�г���ʱ��Ӧ��add_executable�г������ƺ���ӹؼ��� WIN32 ��

### �ؼ��� project

������������ÿ��cmake��Ŀ��Ӧ�������˹ؼ��֣���Ӱ����������Ŀ�����ԡ�cmakeҲ���ṩ��Ŀ��Ӧ�ĸ������������磺

- PROJECT_NAME ��Ŀ����
- PROJECT_SOURCE_DIR ��ĿԴ���Ŀ¼
- PROJECT_VERSION ��Ŀ�汾
- PROJECT_BINARY_DIR ��Ŀ���ɵ���ʱ������Ŀ¼�����ڴ������/�����м��ļ���

### �ؼ���target

target��cmake����һ������Ҫ�ĸ����Ӧ���������Ϊһ��object���������������������ݣ�

- ��ص�Դ�ļ��б�
- ��صı���ѡ��
- ��ص�������
- ��ص�ͷ�ļ�·���б�
- ��صĿ��ļ�·���б�
- ��ص���������

���ԣ����ں�������ʹ�����target�������κ����顣��Ӧ��cmake������ʹ�ø������Զ���ȡ������ʹ�õ�����ֵ��

### �ؼ���PUBLIC PRIVATE INTERFACE

��Щ�ؼ��������ڴ������ϵ

- PUBLIC �����ùؼ��ֺ�����ֵ�ڹ�����targetʱʹ�ã����������ṩ��
- PRIVATE �����ùؼ��ֺ�����ֵ���ڹ�����targetʱʹ�ã����������ṩ��
- INTERFACE �����ùؼ��ֽ��������ṩ�����ڹ�����targetʱʹ�á�

```cmake
cmake_minimum_required(VERSION 3.0)
project(sample CXX)
add_library(sample sample.cpp)
add_executable(sample_exe sample_exe.cpp)
```

## ����

�����в������������еĲ�����

- -S����CMakeLists.txt������project����������·����
- -B�����ʱ����Ķ������ļ���.obj��.ilk�ȣ��ͱ�������Ӧ�������ļ�·����
- -G����������-A�ܹ�����-Dʹ�øñ�������cmake������ֲ���������ѡ�����cmake�ṩ�ĸ���Ĭ�ϱ���ֵ��
- --toolchaincmake toolchain�ļ�·������һ�㽫�ں������⡣
- --install-prefix��װ�Ķ����ƴ��·����
- --trace / --trace-expand����ʱʹ�ã����ڴ�ӡ��ִ�е�cmake���뼰�кš�������������message�е����ݡ�
- --buildʹ��cmakeֱ�ӵ��ñ�����������Ŀ��
- --configѡ����Ҫ�������Ŀ�������͡�
- --install��װ�ѱ���õĶ������ļ��� CMAKE_INSTALL_PREFIX �С�

## ��������

### ��������

find_package
find_packageּ��ʹ��Ԥ�����õ������ļ�������������亯��ԭ��Ϊ��

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

������Ʊ�ʾ����Ҫ���ҵĿ�����ƣ���Сд���У���֮��Ӧ���ǵ����˰��������Ƶ������ļ������ʹ�øú���ʼ���Ҳ����⣬����ϸ�鿴�ļ������а����Ŀ����ƴ�Сд��

- version

�����İ汾�š�

�������������ͬʱ�ṩ�˰汾�ļ������ʹ�ø�ֵ�Ա������еİ汾��ȷ���Ƿ����ʹ�á�

- EXACT

�ùؼ��������˰汾�ű����ϸ��Ӧ�����еİ汾�ź󣬴������ſ��Ա�ʹ�á���һ��Сһ������ɡ�

- QUIET

�ùؼ��ֹر��˲�����Ϣ������������ʧ��/������Ϣ���������

- CONFIG

�ùؼ�����������Ҫʹ�� ������ͨ���Լ���cmake���� ʹ��cmake �Զ����ɵ� �����ļ�����������ļ�����һ��Ϊ <LOW_CASE_PACKAGE_NAME>-config.cmake �� <ALL_CASE_PACKAGE_NAME>Config.cmake ��

��ע��һ����ʹ��ȫСд���������ƶ���һ��ʹ�ð�����Сд�����������ơ���һ��ܶ��˺����ˣ��������Լ���Ŀ��ʼ���Ҳ��������ļ�����ʹ����·����ȷ��

����ʹ�� version �ؼ���ʱ��cmake������ļ� <LOW_CASE_PACKAGE_NAME>-config-version.cmake �� <ALL_CASE_PACKAGE_NAME>ConfigVersion.cmake ���鿴�汾�Ƿ�ƥ�䡣

������ͨ���������cmake�����Զ����ɵģ�һ������²�����ڻ���ִ����������Ƽ�ʹ�ô�ģʽ��

���ڲ���·����˵���������ر����׺����ַǳ���Ҫ��һ�㡣

�ٷ��ĵ��ǣ�find_package - CMake 3.23.0-rc5 Documentation ��

��˵����Ҫע��ļ��㣺

    <PACKAGE_NAME>_DIR ������� find_package ֮ǰֱ�������������������·����������cmake��Ҫ���ҵ�·��������һЩ����ĳ�������Ч��
    CMAKE_PREFIX_PATH cmakeͳһʹ�ø�list�е�·������������������������������������һ���뽫�������ڵĸ�Ŀ¼ APPEND �� REPEND ����list�С�
    CMAKE_FRAMEWORK_PATH ��·��ΪMacOSר�ã����ϵͳ framework ��������λ�á�
    CMAKE_APPBUNDLE_PATH ��·��ΪMacOSר�ã���� app bundle��λ�á�

��������ǰ��������/�б���ԣ�cmake����ÿһ����������չ·���в��������ļ�:

    <EACH_PATH>/ ��Ŀ¼
    <EACH_PATH>/cmake
    <EACH_PATH>/<PACKAGE_NAME>
    <EACH_PATH>/<PACKAGE_NAME>/cmake
    <EACH_PATH>/<lib/share>/cmake
    <EACH_PATH>/<lib/share>/<PACKAGE_NAME>
    <EACH_PATH>/<lib/share>/<PACKAGE_NAME>/cmake
    <EACH_PATH>/<PACKAGE_NAME>/<lib/share>/<PACKAGE_NAME>
    <EACH_PATH>/<PACKAGE_NAME>/<lib/share>/<PACKAGE_NAME>/cmake

�����ǲ��ǣ���ʵͨ�������ܼ򵥣�������Ҫ�ṩ�������ļ��ӵ� root/share/<PACKAGE_NAME> �� root/lib/cmake/<PACKAGE_NAME> �о����ˡ�

ʹ�ø�ģʽʱ��һ������»��ṩ�������Ӧ�� target ���ƣ������򲻰���namespace���Թ�ʹ�á������������Ҳ�ṩ�������Ӧ�ĸ��ֺ꣬����Ҫ��Ϊ�˼���Ԥ��cmake�ṩ��MODULEģʽ�ļ�ʹ�÷�ʽ������CMAKE_FIND_PACKAGE_PREFER_CONFIG �������޸��κ� find_package ���뼴������ʹ�� CONFIG ģʽ����

��δʹ�� REQUIRED �ؼ���ʱ�����ǿ���ʹ�� <PACKAGE_NAME>_FOUND ���ж��Ƿ��Ҵ��˺��ʵ������

�ùؼ��ֲ�������һ�ؼ���ͬʱ������

- MODULE

�ùؼ�����������Ҫʹ�� cmake�ٷ� �� ������� �� ����Ŀ �ṩ�� Find<PACKAGE_NAME>.cmake �ļ���

ʹ������һ����ͬ�Ĳ��ҹ�����Ҫ�ر�ע�����ǣ����ṩ��һ������Ĳ���·�� CMAKE_MODULE_PATH ������Խ��Լ�д�õ������ļ�·�� PREPEND ����� list��������ʹ����������ļ���

����ʹ�� version �ؼ���ʱ��cmake��ʹ����������ĳЩ�ļ�������ͷ�ļ���README���д��ڵİ汾�Ŷ�Ӧ�����ļ��еĺ� <PACKAGE_NAME>_VERSION ���鿴�汾�Ƿ�ƥ�䡣

ʹ�ø�ģʽʱ��һ������»��ṩ�������Ӧ�ĺ��Թ�ʹ�ã�

    <PACKAGE_NAME>_INCLUDE_DIRS / <PACKAGE_NAME>_INCLUDE_DIR ͷ�ļ�·��
    <PACKAGE_NAME>_LIBRARIES / <PACKAGE_NAME>_LIBRARY �����ƣ�����·�������ñ��ʽ��
    <PACKAGE_NAME>_VERSION / <PACKAGE_NAME>_VERSION_STRING �����汾��
    <PACKAGE_NAME>_FOUND �Ƿ���ҵ�������

�������������Ҳ�ṩ target �����Թ�����ʹ�á�

ע�⣺��û��������һ��������ؼ���ʱ��cmake����� CMAKE_FIND_PACKAGE_PREFER_CONFIG ���������ж�����ʹ�� CONFIG ģʽ�� MODULE ģʽ��

- NAMES

���˹ؼ��ֺ���������滻PACKAGE_NAME_CASE_SENSITIVE�����������ļ������ṩ�����ƶ�Ӧ�ĺꡣ

���ڴ���S��������Զ�д������ƥ�䡣

- PATHS

�������������ļ��ĸ�Ŀ¼���൱�� CMAKE_PREFIX_PATH�� �����Ƕ���ġ�

- PATH_SUFFIXES

�������Ҹ�Ŀ¼�µĶ�����չ���·����

- NO_DEFAULT_PATH

��ʹ��Ĭ��·�����ң�Ҳ����˵��ʹ�������оٵ��� CMAKE_ Ϊǰ׺��·��������������<PAKCAGE_NAME>_DIR �� PATHS �����ҡ�

��һ���ڽ���cmake�Դ���MODULE�ļ�ʱ����Ч��

- NO_SYSTEM_PATH

����ϵͳ·���в��ҡ�
find_library

����ԭʼ��cmake����������ʽ��ֱ�Ӳ�����������ļ���һ���������һ��ͬʱʹ�á�

�亯��ԭ��Ϊ��

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

��������� find_package ���в�ͬ���ҽ��ڴ�˵����֮ͬ����

    LIBRARY_NAME ����ֱ�Ӳ��ҿ��ļ������ǲ��������ļ��������ƽ���Ϊ����к��ǰ׺ʹ�á�
    NAMES ���������˿��ļ������ơ�ֵ��ע����ǣ���UNIX-styleϵͳ�У��Զ���ӡ�lib����Ϊ�����Ƶ�ǰ׺��
    NAMES_PER_DIR һ�����Ʊ�������һ�Σ�������һ�����Ʊ�������һ�Ρ������Ǹ���·��ʹ�ö�����Ʊ�����

������ɺ�

    ������ҵ���������� LIBRARY_NAME Ϊ���ҵ��Ŀ��ļ������ƣ�����ȫ·������
    ���û�в��ҵ�����Ὣ LIBRARY_NAME ����Ϊ <LIBRARY_NAME>-NOTFOUND ��

��������� find_package ���в�ͬ������Ӧ��ʹ�����´����ж��Ƿ���ҵ�:

if (PACKAGE_NAME MATCHES "-NOTFOUND")
    message(FATAL_ERROR "${PACKAGE_NAME} not found!")
endif()

find_path

�������һ���ǲ���ͷ�ļ��������� �ǿ��ļ� �� �ǿ�ִ�г����亯��ԭ��Ϊ��

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

����ûʲô��˵�ģ��ο���һ����

һ������£�������Ҫcmake���ʽ����cmake�ж�ʹ���ĸ����õĿ⣬����ͨ����ôд��

```
find_path(<PACKAGE_NAME>_INCLUDE_DIR NAMES header.h PATH_SUFFIXES include/...)

find_library(<PACKAGE_NAME>_LIBRARY_RELEASE NAMES name1 name2)
find_library(<PACKAGE_NAME>_LIBRARY_DEBUG NAMES name1d name2d)
select_library_configurations(<PACKAGE_NAME>)
...
target_*(target_name ${<PACKAGE_NAME>})

find_program
```

�������ר�����ڲ��ҿ�ִ�г����亯��ԭ��Ϊ��

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
Ҳûʲô��˵�ģ�ֱ��ʹ�� PROGRAM_NAME �ͺ��ˡ�
ʹ������

����������Ŀ����ը���������ڿ���ʹ���������ˡ����ǿ��Խ����ҵ������������ڶ�������У��������ͷ�ļ�·����������ӿ⣬��ӱ���ѡ��ȡ�

���ڲ�ͬ�Ĳ��ҷ�ʽ�������ļ���cmake�ṩ�˲�ͬ��ʹ�÷�ʽ��
��

���� `<PACKAGE_NAME>_INCLUDE_DIRS` �� `<PACKAGE_NAME>_LIBRARIES` ���ַ�ʽ��

����ͷ�ļ�������ֱ�Ӽӵ�include_directories�оͺ��ˡ������ڿ����������ӵ㣺

���ڲ��ܻ��ʹ��debug�⼰release�⣬cmake������ȷ֪���ڲ�ͬ������ʹ���ĸ��⡣���Ժ���һ��ʹ�õ���cmake���ʽ���������������cmake-generator-expressions(7) - CMake 3.23.0-rc5 Documentation

���磺

```
$<$<CONFIG:DEBUG>:library.lib> $<${NOT:$<CONFIG:DEBUG>>:libraryd.lib>
```
����������д����ʱ��������debug��release������Һ�ʹ�� select_library_configurations �����ɱ��ʽ�Ա㲻ͬ������ʹ�á�
target

target �ͼ򵥵Ķ��ˣ���Ϊ����һ��object��cmake��������������ȡ target ��������Ҫʹ�õ�������ʹ�á�

��Ȼ��target ������namespace��namespace������ʽ������ʹ����û����
�ڲ�����

���ڴ�����Ŀ��˵�����ǿ�����Ҫ������ͬ��target��������Щtarget����������ϵ����������ͨ��ʹ�����·�ʽ��
add_dependencies

����ԭ��Ϊ��

add_dependencies(<target> [<target-dependency>]...)

����ܼ򵥣���ǰ�������������ߣ���������Ӷ�����ڱ����ĳЩ����ʱ�����ȴ�����ߡ�