cmake_minimum_required(VERSION 3.5.1) 

project(predmetOpresnikVajaX VERSION 0.1 LANGUAGES CXX)


set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if(CMAKE_COMPILER_IS_GNUCC OR CMAKE_COMPILER_IS_GNUCXX)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-long-long")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pedantic")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Werror=return-type")
endif()

# find_package(OpenMP)
# if (OPENMP_FOUND)
#     set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
#     set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
# endif()

# Set the output folder where your program will be created
set(CMAKE_BINARY_DIR ${CMAKE_BINARY_DIR})
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR})
set(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR})

# Lib ---------------------------------


# Source files to be used in the library
set(libstart_SOURCES
    src/libstart.cpp
)

#Generate the static library from the library sources
add_library(libstart STATIC ${libstart_SOURCES})

target_include_directories(libstart
    PUBLIC ${PROJECT_SOURCE_DIR}/include
)


# External libs -----------------------

# find_package(OpenCV 2.4.3 REQUIRED ) # in 3.0: require videoio? Plus the thing Michael fixed for eos?
# message(STATUS "OpenCV include dir found at ${OpenCV_INCLUDE_DIRS}")
# message(STATUS "OpenCV library dir found at ${OpenCV_LIB_DIR}")
# set_target_properties(${OpenCV_LIBS} PROPERTIES MAP_IMPORTED_CONFIG_RELWITHDEBINFO RELEASE)

# Program -----------------------------

set(vaja_SOURCES 
    src/main.cpp
    )

add_executable("${CMAKE_PROJECT_NAME}" ${vaja_SOURCES})

target_link_libraries("${CMAKE_PROJECT_NAME}"
    PRIVATE libstart
#     PRIVATE ${OpenCV_LIBS}
    )

    
# enable_testing()
# add_subdirectory("${CMAKE_SOURCE_DIR}/tests")
 
