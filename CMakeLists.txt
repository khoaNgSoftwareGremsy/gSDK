project(gSDK)
option(TEST_${PROJECT_NAME} "build application to test" OFF)
cmake_minimum_required(VERSION 3.5)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

set(MAVLINK_INCLUDES
    "${CMAKE_CURRENT_SOURCE_DIR}/src/mavlink/include/mavlink/v2.0")

set(MAVLINK_SRC
    "${CMAKE_CURRENT_SOURCE_DIR}/src")

###############################################################################
#### INCLUDES #################################################################
###############################################################################

include_directories(${CMAKE_CURRENT_SOURCE_DIR}
                    ${CMAKE_CURRENT_SOURCE_DIR}/src
                    ${MAVLINK_INCLUDES}
                    )

###############################################################################
#### DEPENDENCIES #############################################################
###############################################################################

###############################################################################
#### GENERATE OUTPUT ##########################################################
###############################################################################
if(TEST_${PROJECT_NAME})
    set(GSDK_APP_SRC
        mavlink_control.cpp
        ${MAVLINK_SRC}/parsers/utils.cpp
        ${MAVLINK_SRC}/bootloader.cpp
        ${MAVLINK_SRC}/gimbal_interface.cpp
        ${MAVLINK_SRC}/gimbal_protocol.cpp
        ${MAVLINK_SRC}/gimbal_protocol_v1.cpp
        ${MAVLINK_SRC}/gimbal_protocol_v2.cpp
        ${MAVLINK_SRC}/serial_port.cpp
    )

    add_executable(gSDK ${GSDK_APP_SRC})

    find_package( Threads )
    target_link_libraries(gSDK
                        ${CMAKE_THREAD_LIBS_INIT}
                        )
    install(TARGETS
        gSDK
        RUNTIME DESTINATION usr/bin/pixy_payload)
else()
    set(GSDK_APP_SRC
        ${MAVLINK_SRC}/parsers/utils.cpp
        ${MAVLINK_SRC}/bootloader.cpp
        ${MAVLINK_SRC}/gimbal_interface.cpp
        ${MAVLINK_SRC}/gimbal_protocol.cpp
        ${MAVLINK_SRC}/gimbal_protocol_v1.cpp
        ${MAVLINK_SRC}/gimbal_protocol_v2.cpp
        ${MAVLINK_SRC}/serial_port.cpp
    )
    add_library(${PROJECT_NAME}
            STATIC
            ${GSDK_APP_SRC})
    target_include_directories(${PROJECT_NAME} 
                                PUBLIC
                                ${MAVLINK_INCLUDES}
                                ${CMAKE_CURRENT_SOURCE_DIR}
                                ${CMAKE_CURRENT_SOURCE_DIR}/src
    )
endif()


# add_subdirectory("${CMAKE_CURRENT_SOURCE_DIR}/examples")