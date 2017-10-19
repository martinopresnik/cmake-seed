#Setup CMake to run tests
enable_testing()

#Prep ourselves for compiling boost
find_package(Boost COMPONENTS unit_test_framework REQUIRED)
include_directories (${Boost_INCLUDE_DIRS})

include_directories("${CMAKE_SOURCE_DIR}")

#I like to keep test files in a separate source directory called test
# file(GLOB TEST_SRCS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} *.cpp)
set(TEST_SRCS
    "initTest.cpp"
)


set(MyLibs
    libstart
)

set(testFiles
    
)

foreach(testFile ${testFiles})
    file(COPY "${CMAKE_CURRENT_SOURCE_DIR}/testFiles/${testFile}" DESTINATION "${CMAKE_CURRENT_BINARY_DIR}/testFiles/")
endforeach()

# file(COPY "${CMAKE_CURRENT_SOURCE_DIR}/testFiles" DESTINATION "${CMAKE_CURRENT_BINARY_DIR}")

#Run through each source
foreach(testSrc ${TEST_SRCS})
        #Extract the filename without an extension (NAME_WE)
        get_filename_component(testName ${testSrc} NAME_WE)

    #Add compile target
	add_executable(${testName} ${testSrc})
	
    #link to Boost libraries AND your targets and dependencies
	target_link_libraries(${testName} 
        ${Boost_LIBRARIES}
#         ${OpenCV_LIBS} 
        ${MyLibs}
	)

        #I like to move testing binaries into a testBin directory
        set_target_properties(${testName} PROPERTIES 
            RUNTIME_OUTPUT_DIRECTORY  ${CMAKE_CURRENT_BINARY_DIR}
            )

        #Finally add it to test execution - 
        #Notice the WORKING_DIRECTORY and COMMAND
        add_test(NAME ${testName}
                 WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
                 COMMAND ${CMAKE_CURRENT_BINARY_DIR}/${testName} )
endforeach(testSrc)


