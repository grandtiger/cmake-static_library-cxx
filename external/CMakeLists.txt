##########################################################################################
# ┌──────────────────────────────────────────────────────────────────────────────────────┐
# │                                                                                      │
# │      CMake build script                                                              │
# │  * * * * * * * * * * * * *                                                           │
# │                                                                                      │
# └──────────────────────────────────────────────────────────────────────────────────────┘
##########################################################################################
##########################################################################################
cmake_minimum_required(VERSION 3.13.2 FATAL_ERROR)


##########################################################################################
# ┌──────────────────────────────────────────────────────────────────────────────────────┐
# │  CMake status output                                                                 │
# └──────────────────────────────────────────────────────────────────────────────────────┘
##########################################################################################
message(STATUS "CMake version   : " "${CMAKE_SYSTEM_VERSION}"    )
message(STATUS "Operating System: " "${CMAKE_SYSTEM}"            )
message(STATUS "I am building   : " "${PROJECT_NAME}"            )

if (CMAKE_CXX_COMPILER_LOADED)
    message(STATUS "Compiler loaded : " "${CMAKE_CXX_COMPILER_LOADED}" )
    message(STATUS "Compiler type   : " "${CMAKE_CXX_COMPILER_ID}"     )
    message(STATUS "Compiler version: " "${CMAKE_CXX_COMPILER_VERSION}")
    message(STATUS "Compiler path   : " "${CMAKE_CXX_COMPILER}"        )

else()
    message(FATAL_ERROR "No compiler for the job, sorry.")

endif()



##########################################################################################
# ┌──────────────────────────────────────────────────────────────────────────────────────┐
# │  CMake include external libraries and frameworks                                     │
# └──────────────────────────────────────────────────────────────────────────────────────┘
##########################################################################################
if (BUILD_TESTING)
    add_subdirectory(cmock)
    add_subdirectory(unity)

endif()

add_subdirectory(c_exception)
