SET(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})

CMAKE_MINIMUM_REQUIRED(VERSION 2.6 FATAL_ERROR)
CMAKE_POLICY(VERSION 2.6)

FIND_PACKAGE(Torch REQUIRED)

SET(BUILD_STATIC YES) # makes sure static targets are enabled in ADD_TORCH_PACKAGE

SET(CMAKE_C_FLAGS "--std=c99 -pedantic -Werror -Wall -Wextra -Wno-unused-function -D_GNU_SOURCE ${CMAKE_C_FLAGS}")
SET(src
   src/thrift.c
)

SET(luasrc
   test/test.lua
)

ADD_TORCH_PACKAGE(thrift "${src}" "${luasrc}" "Thrift serialization for Torch")

TARGET_LINK_LIBRARIES(thrift luaT TH)

SET_TARGET_PROPERTIES(thrift_static PROPERTIES COMPILE_FLAGS "-fPIC -DSTATIC_TH")

INSTALL(FILES ${luasrc} DESTINATION "${Torch_INSTALL_LUA_PATH_SUBDIR}/thrift")
