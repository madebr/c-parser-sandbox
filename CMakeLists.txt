cmake_minimum_required(VERSION 3.12...3.30 FATAL_ERROR)

file(READ "${CMAKE_CURRENT_SOURCE_DIR}/version.txt" version_txt)
string(STRIP "${version_txt}" VERSION)

project(sandbox LANGUAGES C VERSION "${VERSION}")

find_package(Python3 REQUIRED COMPONENTS Interpreter Development)

Python3_add_library(_sandbox MODULE
    src/sandbox.c
    src/tokenizer.c
)

if(DEFINED PY_BUILD_CMAKE_VERSION)
    set(MODULE_INSTALL_DESTINATION "${PY_BUILD_CMAKE_IMPORT_NAME}")
else()
    set(MODULE_INSTALL_DESTINATION "${Python3_SITEARCH}")
endif()


install(TARGETS _sandbox EXCLUDE_FROM_ALL
    LIBRARY DESTINATION "${MODULE_INSTALL_DESTINATION}"
    COMPONENT python_modules
)
