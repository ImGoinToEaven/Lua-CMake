cmake_minimum_required(VERSION 3.15)
project(Lua LANGUAGES C)

# Specify the Lua version (optional)
set(LUA_VERSION "5.4.6" CACHE STRING "Lua version")

# List all Lua source files
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
        src/lua/loslib.c
        src/lua/lstrlib.c
        src/lua/ltablib.c
        src/lua/loadlib.c
        src/lua/linit.c
)

# List all Lua header files
set(LUA_HEADERS
        src/lua/lua.h
        src/lua/lauxlib.h
        src/lua/lualib.h
)

# Create a static library for Lua
add_library(Lua STATIC
        ${LUA_SOURCES}
)

# Specify include directories
target_include_directories(Lua PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
        $<INSTALL_INTERFACE:include>
)

# Set C standard to C99 (Lua is written in C)
set_target_properties(Lua PROPERTIES
        C_STANDARD 99
        PUBLIC_HEADER "${LUA_HEADERS}"
)

# (Optional) Define any necessary compilation options or macros
# For example, to define LUA_COMPAT_ALL if needed:
# target_compile_definitions(Lua PRIVATE LUA_COMPAT_ALL)

# Link libraries if necessary (Lua typically doesn't require additional libraries)
# target_link_libraries(Lua PRIVATE )

# Installation rules
install(TARGETS Lua
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
        RUNTIME DESTINATION bin
        PUBLIC_HEADER DESTINATION include/lua
)

install(DIRECTORY src/lua/
        DESTINATION include/lua
        FILES_MATCHING PATTERN "*.h"
)
