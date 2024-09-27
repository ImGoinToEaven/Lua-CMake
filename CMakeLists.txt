# Minimum CMake version required
cmake_minimum_required(VERSION 3.15)

# Project declaration
project(lua_static
        VERSION 5.4.6
        DESCRIPTION "Lua Static Library"
        LANGUAGES C
)

# Specify the Lua source directory
set(LUA_SRC_DIR "${CMAKE_CURRENT_SOURCE_DIR}/src/lua")

# Collect all Lua source files
set(LUA_SOURCES
        ${LUA_SRC_DIR}/lapi.c
        ${LUA_SRC_DIR}/lcode.c
        ${LUA_SRC_DIR}/lctype.c
        ${LUA_SRC_DIR}/ldebug.c
        ${LUA_SRC_DIR}/ldo.c
        ${LUA_SRC_DIR}/ldump.c
        ${LUA_SRC_DIR}/lfunc.c
        ${LUA_SRC_DIR}/lgc.c
        ${LUA_SRC_DIR}/llex.c
        ${LUA_SRC_DIR}/lmem.c
        ${LUA_SRC_DIR}/lobject.c
        ${LUA_SRC_DIR}/lopcodes.c
        ${LUA_SRC_DIR}/lparser.c
        ${LUA_SRC_DIR}/lstate.c
        ${LUA_SRC_DIR}/lstring.c
        ${LUA_SRC_DIR}/ltable.c
        ${LUA_SRC_DIR}/ltm.c
        ${LUA_SRC_DIR}/lundump.c
        ${LUA_SRC_DIR}/lvm.c
        ${LUA_SRC_DIR}/lzio.c
        ${LUA_SRC_DIR}/lauxlib.c
        ${LUA_SRC_DIR}/lbaselib.c
        ${LUA_SRC_DIR}/lcorolib.c
        ${LUA_SRC_DIR}/ldblib.c
        ${LUA_SRC_DIR}/liolib.c
        ${LUA_SRC_DIR}/lmathlib.c
        ${LUA_SRC_DIR}/loslib.c
        ${LUA_SRC_DIR}/lstrlib.c
        ${LUA_SRC_DIR}/ltablib.c
        ${LUA_SRC_DIR}/loadlib.c
        ${LUA_SRC_DIR}/linit.c
)

# Collect all Lua header files
set(LUA_HEADERS
        ${LUA_SRC_DIR}/lua.h
        ${LUA_SRC_DIR}/lauxlib.h
        ${LUA_SRC_DIR}/lualib.h
)

# Define the static library target
add_library(lua_static STATIC ${LUA_SOURCES})

# Set the C standard to C99
set_target_properties(lua_static PROPERTIES
        C_STANDARD 99
        C_STANDARD_REQUIRED YES
        C_EXTENSIONS NO
        OUTPUT_NAME lua_static
)

# Specify include directories
target_include_directories(lua_static PUBLIC
        $<BUILD_INTERFACE:${LUA_SRC_DIR}>
        $<INSTALL_INTERFACE:include/lua>
)

# (Optional) Define any necessary compilation options or macros
# For example, to define LUA_COMPAT_ALL if needed:
# target_compile_definitions(lua_static PRIVATE LUA_COMPAT_ALL)

# Prevent Lua from building its own executable (optional)
# If Lua's `lua.c` is present and you want to exclude it from the library,
# make sure it's not listed in LUA_SOURCES.

# Note: Since we're building a static library, there's no need to link against additional libraries.

# (Optional) Add compile options for better warnings and optimizations
# Uncomment and modify as needed
# target_compile_options(lua_static PRIVATE -Wall -Wextra -O2)

# (Optional) Set position-independent code if required
# set_target_properties(lua_static PROPERTIES POSITION_INDEPENDENT_CODE ON)

# No install rules or export targets are defined to keep it simple
