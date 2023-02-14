
cmake_minimum_required(VERSION 3.5.1)

project(sparkassistant LANGUAGES CXX VERSION 1.0.0)

include(cmake/SparkEnvConfig.cmake)
include(cmake/SparkMacrosConfig.cmake)
include(cmake/SparkFindLibraries.cmake)
include(cmake/SparkFindQt5Config.cmake)
include(cmake/SparkFindQt6Config.cmake)
include(cmake/SparkFindDtkConfig.cmake)
include(cmake/SparkInstallMacrosConfig.cmake)

# 资源文件路径
set(QRC_SOURCES "assets/resources.qrc")

# 将 src 目录下的源代码 与 qrc 文件进行构建
spark_add_executable_path(${PROJECT_NAME} src
    ${QRC_SOURCES}
)
target_link_qt5(${PROJECT_NAME})
target_link_dtk(${PROJECT_NAME})


include(cmake/SparkDesktopMacros.cmake)
# 内容默认应用名称: Name= 应与项目名称相同
spark_desktop_macros(
    # 应用名称: Name=
    ${PROJECT_NAME}
    # 应用名称: Name[zh_CN]=
    "星火小助手"
    # 应用说明: Comment=
    ""
    # 应用类型: Type=
    "Application"
    # 执行程序: Exec=
    "/usr/bin/sparkassistant %F"
    # 图标路径: Icon=
    "/usr/share/icons/spark-store.png"
    # 应用分类: Category=
    ""
)
# spark_install_file
# 基于传入的路径/文件进行安装
# 可接受的值为: 安装路径 文件A
spark_install_file(/usr/share/applications/
    ${SPARK_DESKTOP_FILE})  # 安装由 spark_desktop_macros 生成的 desktop 文件

spark_install_file(/usr/share/icons/
    assets/icon/spark-store.png) # 安装图标文件

# spark_install_target
# 基于传入的路径/目标进行安装
# 可接受的值为: 安装路径 目标A
spark_install_target(/usr/bin/
    ${PROJECT_NAME})    # 安装可执行目标程序


# 安装 changelog
spark_install_changelog(${CMAKE_SOURCE_DIR}/debian/changelog)

include(cmake/SparkBuildGraphviz.cmake)

# deb 软件构建注释行(使用方式)
find_package(DebPackage PATHS ${CMAKE_SOURCE_DIR})
add_package_descript(cmake/package-deb.descript)