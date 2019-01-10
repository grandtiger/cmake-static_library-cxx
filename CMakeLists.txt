#########################################################################################
#
#
#	 CMake build script
#	== == == == == == == 
#
#
#########################################################################################
cmake_minimum_required(VERSION 3.13.2 FATAL_ERROR)



##########################################################################################
# ┌──────────────────────────────────────────────────────────────────────────────────────┐
# │  CMake check build type                                                              │
# └──────────────────────────────────────────────────────────────────────────────────────┘
##########################################################################################
option(CMAKE_BUILD_TYPE "Setting build type to release build"  OFF)
option(CMAKE_BUILD_TYPE "Setting build type to debug build"    ON )
option(BUILD_TESTING    "Setting build type to test build "    ON )



##########################################################################################
# ┌──────────────────────────────────────────────────────────────────────────────────────┐
# │  CMake check build type                                                              │
# └──────────────────────────────────────────────────────────────────────────────────────┘
##########################################################################################
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug CACHE STRING "Build type" FORCE)
endif()
#end


project(sqdList
    VERSION 1.0.0 
    LANGUAGES CXX C 
    DESCRIPTION "A working doubly linked list written in C.  " 
    HOMEPAGE_URL "https://github.com/sqdfarts/cmake-sqdlist-c"
)

##########################################################################################
# ┌──────────────────────────────────────────────────────────────────────────────────────┐
# │  CMake include modules                                                               │
# └──────────────────────────────────────────────────────────────────────────────────────┘
##########################################################################################
# This helps CMake with GNU install paths
include(GNUInstallDirs)

# This helps CMake with config files
include(CMakePackageConfigHelpers)

# Enable tests as long as BUILD_TESTING is ON
include(CTest)

# Enable LTO on Release builds
include(CheckIPOSupported)



##########################################################################################
# ┌──────────────────────────────────────────────────────────────────────────────────────┐
# │  CTest setting up support                                                            │
# └──────────────────────────────────────────────────────────────────────────────────────┘
##########################################################################################
check_ipo_supported(RESULT ipo_supported)
if(ipo_supported)
    set(CMAKE_INTERPROCEDURAL_OPTIMIZATION_RELEASE ON)
endif()

# Enable running and testing on Windows under Debug
# To test other configs, just `install' to a local dir before testing
if(WIN32)
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG "${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_BINDIR}")
endif()



##########################################################################################
# ┌──────────────────────────────────────────────────────────────────────────────────────┐
# │  CMake add library sub directories                                                   │
# └──────────────────────────────────────────────────────────────────────────────────────┘
##########################################################################################
add_subdirectory(external)
add_subdirectory(src     )

if(BUILD_TESTING)
    add_subdirectory(test)
endif()



##########################################################################################
# ┌──────────────────────────────────────────────────────────────────────────────────────┐
# │  CMake install executable /CPack config file                                         │
# └──────────────────────────────────────────────────────────────────────────────────────┘
##########################################################################################

# Install the targets, exporting the interface
install(TARGETS "${PROJECT_NAME}" EXPORT "${PROJECT_NAME}-targets"
    RUNTIME
        DESTINATION "${CMAKE_INSTALL_BINDIR}"
        COMPONENT Runtime
    LIBRARY
        DESTINATION "${CMAKE_INSTALL_LIBDIR}"
        COMPONENT Runtime
    ARCHIVE
        DESTINATION "${CMAKE_INSTALL_LIBDIR}"
        COMPONENT Runtime
    INCLUDES DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/include"
)

# Install the public headers when installing the Development component
install(DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/include/"
    COMPONENT Development
    DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}"
    FILES_MATCHING PATTERN "*.h"
)




##########################################################################################
# ┌──────────────────────────────────────────────────────────────────────────────────────┐
# │  CPack setting up support                                                            │
# └──────────────────────────────────────────────────────────────────────────────────────┘
##########################################################################################
set(CPACK_PACKAGE_NAME "${PROJECT_NAME}"                                )
set(CPACK_PACKAGE_VENDOR "Squid Developer labs"                         )
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "Doubly linked list"              )
set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/LICENSE.md")
set(CPACK_RESOURCE_FILE_README  "${CMAKE_CURRENT_SOURCE_DIR}/README.md" )



##########################################################################################
# ┌──────────────────────────────────────────────────────────────────────────────────────┐
# │  CPack include module                                                                │
# └──────────────────────────────────────────────────────────────────────────────────────┘
##########################################################################################
include(CPack)

