cmake_minimum_required(VERSION 3.0)

project(MinimalQtApp VERSION 0.0.1)

set(QTDIR $ENV{QTDIR})
set(CMAKE_PREFIX_PATH ${QTDIR})
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)

if(APPLE)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -stdlib=libc++ -mmacosx-version-min=10.8")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -headerpad_max_install_names")
elseif(UNIX)
add_definitions(-std=c++11)
endif()

find_package(Qt5Widgets REQUIRED)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${CMAKE_CURRENT_BINARY_DIR})

# Sources
file(GLOB SOURCES "*.cpp")
file(GLOB HEADERS "*.h")
file(GLOB UI_SOURCES "*.ui")

qt5_wrap_ui(UI_HEADERS ${UI_SOURCES})

if(WIN32)
set(APP_ICON ${CMAKE_CURRENT_SOURCE_DIR}/icon.rc)
elseif(APPLE)
set(MACOSX_BUNDLE_ICON icon.icns)
set(APP_ICON ${CMAKE_CURRENT_SOURCE_DIR}/icon.icns)
set_source_files_properties(${APP_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")
endif()

if(WIN32)
set(APP_BUNDLE WIN32)
elseif(APPLE)
set(APP_BUNDLE MACOSX_BUNDLE)
endif()

add_executable(${PROJECT_NAME} ${APP_BUNDLE} ${APP_ICON} ${SOURCES} ${HEADERS} ${UI_HEADERS})

if(APPLE)
set_target_properties(${PROJECT_NAME} PROPERTIES MACOSX_BUNDLE_INFO_PLIST ${CMAKE_CURRENT_SOURCE_DIR}/AppleInfo.plist)
endif()

target_link_libraries(${PROJECT_NAME} Qt5::Widgets)
