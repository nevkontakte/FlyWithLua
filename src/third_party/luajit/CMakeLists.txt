cmake_minimum_required(VERSION 3.9)

set(LUAJIT_SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR}/upstream/src)

include(ExternalProject)

ExternalProject_Add(project_luajit
  URL http://luajit.org/download/LuaJIT-2.0.1.tar.gz
  PREFIX ${CMAKE_CURRENT_BINARY_DIR}/luajit-2.0.1
  CONFIGURE_COMMAND ""
  BUILD_COMMAND make
  INSTALL_COMMAND make install
  PREFIX=${CMAKE_CURRENT_BINARY_DIR}/luajit-2.0.1
)

ExternalProject_Get_Property(project_luajit install_dir)

add_library(luajit STATIC IMPORTED)

set_property(TARGET luajit PROPERTY IMPORTED_LOCATION ${install_dir}/lib/libluajit-5.1.a)

add_dependencies(luajit project_luajit)

add_executable(myapp main.c)

include_directories(${install_dir}/include/luajit-2.0)

target_link_libraries(myapp luajit)
