file(GLOB HEADER_FILES "./include/bgfx/*.h")
file(GLOB SRC_FILES "./src/*.cpp" "./src/*.h")
list(FILTER SRC_FILES EXCLUDE REGEX ".*src/amalgamated\\.cpp$")
#message(STATUS ${SRC_FILES})

set(TGT "bgfx")

add_library(${TGT} STATIC ${HEADER_FILES} ${SRC_FILES})

set(BX_COMPAT_INCLUDE_DIR "../bx/include/compat/msvc")
set(DEFS "")

if (MSVC)
	set(BX_COMPAT_INCLUDE_DIR "../bx/include/compat/msvc")
	set(DEFS ${DEFS} _CRT_SECURE_NO_WARNINGS -D__STDC_FORMAT_MACROS)
endif()

if (CMAKE_BUILD_TYPE MATCHES DEBUG)
	set(DEFS ${DEFS} -DBX_CONFIG_DEBUG=1 )
else ()
	set(DEFS ${DEFS} -DBX_CONFIG_DEBUG=0 )
endif()

target_include_directories(${TGT} PRIVATE "./3rdparty" "./3rdparty/dxsdk/include" "./3rdparty/khronos" "./include" "../bimg/include" "../bx/include" ${BX_COMPAT_INCLUDE_DIR})
target_compile_definitions(${TGT} PRIVATE ${DEFS})
