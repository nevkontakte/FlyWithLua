cmake_minimum_required(VERSION 3.9)

include(ExternalProject)

project(luajit)

add_dependencies(FlyWithLua project_luajit)

set (luajit_version $ENV{LUAJIT_VERSION})

if ("${lua_version}" STREQUAL "")
  set(luajit_version 2.0.1)
endif()

if (UNIX AND NOT APPLE)
ExternalProject_Add(project_luajit
  URL http://luajit.org/download/LuaJIT-${luajit_version}.tar.gz
  PREFIX ${CMAKE_CURRENT_BINARY_DIR}/luajit-${luajit_version}
  CONFIGURE_COMMAND ""
  PATCH_COMMAND patch -p1 -t -N < ${CMAKE_CURRENT_SOURCE_DIR}/v2.0.1_hotfix1_X-Plane.patch
  BUILD_COMMAND make
  INSTALL_COMMAND ""
  BUILD_IN_SOURCE 1
)

elseif (WIN32)
ExternalProject_Add(project_luajit
  URL http://luajit.org/download/LuaJIT-${luajit_version}.tar.gz
  PREFIX ${CMAKE_CURRENT_BINARY_DIR}/luajit-${luajit_version}
  CONFIGURE_COMMAND ""
  PATCH_COMMAND patch -p1 -t -N < ${CMAKE_CURRENT_SOURCE_DIR}/v2.0.1_hotfix1_X-Plane.patch
  BUILD_COMMAND make CC="gcc" CROSS=x86_64-w64-mingw32- TARGET_SYS=Windows
  INSTALL_COMMAND ""
  BUILD_IN_SOURCE 1
)

elseif (APPLE)
ExternalProject_Add(project_luajit
  URL http://luajit.org/download/LuaJIT-${luajit_version}.tar.gz
  PREFIX ${CMAKE_CURRENT_BINARY_DIR}/luajit-${luajit_version}
  CONFIGURE_COMMAND ""
  PATCH_COMMAND patch -p1 -t -N < ${CMAKE_CURRENT_SOURCE_DIR}/v2.0.1_hotfix1_X-Plane.patch
  BUILD_COMMAND make
  INSTALL_COMMAND ""
  BUILD_IN_SOURCE 1
)
endif ()

add_custom_command(
        TARGET project_luajit POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
                ${CMAKE_CURRENT_BINARY_DIR}/luajit-${luajit_version}/src/project_luajit/src/libluajit.a
                ${CMAKE_CURRENT_BINARY_DIR}/libluajit-5.1.a)   

