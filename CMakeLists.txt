cmake_minimum_required(VERSION 3.23)
cmake_policy(SET CMP0054 NEW)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(PROJECT_NAME "ForgeAPI")
project(${PROJECT_NAME})

set(INCLUDE_PATH include)
set(SOURCE_PATHS ${CMAKE_CURRENT_SOURCE_DIR}/src/)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/${INCLUDE_PATH})
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/src/)

install(DIRECTORY ${INCLUDE_PATH}/ DESTINATION include)

option(COMPILE_TESTS "Enable unit tests" OFF)
option(GENERATE_DOCS "Generate DOCS" OFF)
option(ENABLE_SANITIZERS "Enable sanitizers" ON)
option(ENABLE_THREAD_SANITIZERS "Enable thread sanitizers" OFF)


# Build realted logs
option(VERBOSE "Enable Verbose for logs" OFF)

# Move bin directory to build directory
set(BIN_DESTINATION ${CMAKE_CURRENT_BINARY_DIR}/bin)

if(NOT EXISTS ${BIN_DESTINATION})
    file(MAKE_DIRECTORY ${BIN_DESTINATION})
endif()

message("You can find executable on ${BIN_DESTINATION}")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${BIN_DESTINATION})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${BIN_DESTINATION})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${BIN_DESTINATION})

add_executable(${PROJECT_NAME}
    ${CMAKE_CURRENT_SOURCE_DIR}/main.cpp
)

# ----- LINK LIBRARIES  -----

get_target_property(INCLUDE_DIRS ${PROJECT_NAME} INCLUDE_DIRECTORIES)
message("Include directories for ${PROJECT_NAME}: ${INCLUDE_DIRS}")

target_link_libraries(${PROJECT_NAME} PRIVATE FORGE_API_LIB)
# Enable Sanitizers
if(ENABLE_SANITIZERS)
    target_compile_options(${PROJECT_NAME} PRIVATE -fsanitize=address -fsanitize=undefined)
    target_link_options(${PROJECT_NAME} PRIVATE -fsanitize=address -fsanitize=undefined)
endif(ENABLE_SANITIZERS)

# Unit tests compile
if(COMPILE_TESTS)
    add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/test/)
    enable_testing()

    if(VERBOSE)
        target_compile_definitions(ForgeAPI_TEST PRIVATE VERBOSE)
    endif()

    set(EXPORT_SET "EXPORT_SET")

    # install(TARGETS  YOUR_LIB_NAME
    # EXPORT ${EXPORT_SET}
    # LIBRARY DESTINATION lib
    # ARCHIVE DESTINATION lib
    # RUNTIME DESTINATION bin)

# install(TARGETS gtest gtest_main gmock gmock_main
#     EXPORT ${EXPORT_SET}
#     LIBRARY DESTINATION lib
#     ARCHIVE DESTINATION lib
#     RUNTIME DESTINATION bin
# )

# install(EXPORT ${EXPORT_SET}
#     FILE ${EXPORT_SET}.cmake
#     NAMESPACE MyProject::
#     DESTINATION lib/cmake/MyProject
# )

endif(COMPILE_TESTS)

# Documentation generation based on Doxygen and dot
if(GENERATE_DOCS)
    find_package(Doxygen REQUIRED)
    find_package(Doxygen REQUIRED dot)

    set(DOXYGEN_OUTPUT_DIRECTORY ${BIN_DESTINATION}/Docs/)
    set(DOXYGEN_PROJECT_NAME ${PROJECT_NAME})
    set(DOXYGEN_CALL_GRAPH YES)
    set(DOXYGEN_CALLER_GRAPH YES)
    set(DOXYGEN_WARN_AS_ERROR YES)
    set(DOXYGEN_SOURCE_BROWSER YES)
    set(DOXYGEN_EXTRACT_ALL YES)
    set(DOXYGEN_SORT_MEMBER_DOCS NO)
    set(DOXYGEN_FILE_PATTERNS *.cpp *.h *.hpp)
    set(DOXYGEN_EXCLUDE_PATTERNS */bin/* */build/* */test/* */third_party/* *.cc main.cpp)

    doxygen_add_docs(
        doxygen_doc
        ALL
        ${CMAKE_CURRENT_LIST_DIR}
        COMMENT "Generate doxygen documentation for ${PROJECT_NAME}"
    )

    set(DOXYGEN_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/docs/)
    set(DOXYGEN_GENERATE_HTML YES)
    set(DOXYGEN_GENERATE_MAN NO)
    set(DOXYGEN_EXTRACT_ALL YES)
    message("Doxygen output directory: ${DOXYGEN_OUTPUT_DIRECTORY}")

    doxygen_add_docs(
        doc
        COMMENT "Generating API documentation with Doxygen"
    )
endif(GENERATE_DOCS)