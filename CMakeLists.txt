# CMakeLists.txt for Lua

cmake_minimum_required(VERSION 3.14)
project(Lua VERSION 5.4.7 LANGUAGES C)

# Optionally, set Lua build options here
option(LUA_BUILD_SHARED "Build Lua as a shared library" OFF)
option(LUA_BUILD_TESTS "Build Lua test executables" OFF)
option(LUA_BUILD_COMPILER "Build Lua compiler and executables" OFF)
option(LUA_COMPAT_ALL "Enable all Lua compatibility features" OFF)

# Define the list of Lua source files located in src/lua/
set(LUA_SOURCES
        src/lua/lapi.c
        src/lua/lcode.c
        src/lua/lctype.c
        src/lua/ldebug.c
        src/lua/ldo.c
        src/lua/ldump.c
        src/lua/lfunc.c
        src/lua/lgc.c
        src/lua/llex.c
        src/lua/lmem.c
        src/lua/lobject.c
        src/lua/lopcodes.c
        src/lua/lparser.c
        src/lua/lstate.c
        src/lua/lstring.c
        src/lua/ltable.c
        src/lua/ltm.c
        src/lua/lundump.c
        src/lua/lvm.c
        src/lua/lzio.c
        src/lua/lauxlib.c
        src/lua/lbaselib.c
        src/lua/lcorolib.c
        src/lua/ldblib.c
        src/lua/liolib.c
        src/lua/lmathlib.c
        src/lua/loadlib.c
        src/lua/loslib.c
        src/lua/lstrlib.c
        src/lua/ltablib.c
        src/lua/lutf8lib.c
        src/lua/linit.c
)

# Create a static library
add_library(lua_static STATIC ${LUA_SOURCES})

# Create a shared library if enabled
if(LUA_BUILD_SHARED)
    add_library(lua_shared SHARED ${LUA_SOURCES})
    target_compile_definitions(lua_shared PRIVATE LUA_BUILD_AS_DLL)
endif()

# Define include directories using generator expressions
target_include_directories(lua_static PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
        $<INSTALL_INTERFACE:include>
)

if(LUA_BUILD_SHARED)
    target_include_directories(lua_shared PUBLIC
            $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
            $<INSTALL_INTERFACE:include>
    )
endif()

# Set compile definitions
if(LUA_COMPAT_ALL)
    target_compile_definitions(lua_static PRIVATE LUA_COMPAT_ALL)
    if(LUA_BUILD_SHARED)
        target_compile_definitions(lua_shared PRIVATE LUA_COMPAT_ALL)
    endif()
endif()

# Link libraries based on platform
if(WIN32)
    # Windows-specific libraries
    target_link_libraries(lua_static PRIVATE ws2_32)
    if(LUA_BUILD_SHARED)
        target_link_libraries(lua_shared PRIVATE ws2_32)
    endif()
else()
    # Unix-like systems
    target_link_libraries(lua_static PRIVATE m)
    if(LUA_BUILD_SHARED)
        target_link_libraries(lua_shared PRIVATE m dl)
    endif()
    target_compile_definitions(lua_static PRIVATE LUA_USE_POSIX)
    if(LUA_BUILD_SHARED)
        target_compile_definitions(lua_shared PRIVATE LUA_USE_POSIX LUA_USE_DLOPEN)
    endif()
endif()

# Define the list of public headers for installation
set(LUA_HEADERS
        src/lua/lapi.h
        src/lua/lcode.h
        src/lua/lctype.h
        src/lua/ldebug.h
        src/lua/ldo.h
        src/lua/ldump.h
        src/lua/lfunc.h
        src/lua/lgc.h
        src/lua/llex.h
        src/lua/lmem.h
        src/lua/lobject.h
        src/lua/lopcodes.h
        src/lua/lparser.h
        src/lua/lstate.h
        src/lua/lstring.h
        src/lua/ltable.h
        src/lua/ltm.h
        src/lua/lundump.h
        src/lua/lvm.h
        src/lua/lzio.h
        src/lua/lauxlib.h
        src/lua/lbaselib.h
        src/lua/lcorolib.h
        src/lua/ldblib.h
        src/lua/liolib.h
        src/lua/lmathlib.h
        src/lua/loadlib.h
        src/lua/loslib.h
        src/lua/lstrlib.h
        src/lua/ltablib.h
        src/lua/lutf8lib.h
        src/lua/linit.h
)

# Set properties for the libraries
set_target_properties(lua_static PROPERTIES
        VERSION ${PROJECT_VERSION}
        PUBLIC_HEADER "${LUA_HEADERS}"
        ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
        LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
        RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin"
)

if(LUA_BUILD_SHARED)
    set_target_properties(lua_shared PROPERTIES
            OUTPUT_NAME "lua"
            PREFIX ""
    )
endif()

set_target_properties(lua_static PROPERTIES
        OUTPUT_NAME "lua_static"
        PREFIX ""
)

# Install targets (optional)
install(TARGETS lua_static
        EXPORT LuaTargets
        ARCHIVE DESTINATION lib
        PUBLIC_HEADER DESTINATION include/lua
)

if(LUA_BUILD_SHARED)
    install(TARGETS lua_shared
            EXPORT LuaTargets
            LIBRARY DESTINATION lib
            PUBLIC_HEADER DESTINATION include/lua
    )
endif()

# Export the targets for use in other projects (remove if not needed)
install(EXPORT LuaTargets
        FILE LuaTargets.cmake
        NAMESPACE Lua::
        DESTINATION lib/cmake/Lua
)
