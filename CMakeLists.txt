# The following lines of boilerplate have to be in your project's
# CMakeLists in this exact order for cmake to work correctly
cmake_minimum_required(VERSION 3.16)

if(DEFINED ENV{BUILD_CONFIG})
    set(SDKCONFIG_DEFAULTS "sdkconfig.defaults;sdkconfig.ci.defaults;sdkconfig.ci.$ENV{BUILD_CONFIG}")
endif()

include($ENV{IDF_PATH}/tools/cmake/project.cmake)
project(hello_world)

add_custom_command(TARGET app POST_BUILD
    COMMAND python -m esptool --chip ${chip_model} merge_bin -o ${CMAKE_PROJECT_NAME}-factory.bin @flash_args
)

add_custom_command(TARGET app POST_BUILD
    COMMAND cp ${CMAKE_PROJECT_NAME}.bin ${chip_model}_nosb_${CMAKE_PROJECT_NAME}_${PROJECT_VER}.app.bin
    COMMAND cp ${CMAKE_PROJECT_NAME}-factory.bin ${chip_model}_nosb_${CMAKE_PROJECT_NAME}_${PROJECT_VER}.factory.bin
)