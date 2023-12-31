cmake_minimum_required(VERSION 3.15)

project(QuasarLibrary
  VERSION 2.0.0
  DESCRIPTION "Quasar Library"
  HOMEPAGE_URL "io.github.whs31"
  LANGUAGES C CXX
)

if(NOT CMAKE_C_STANDARD)
    set(CMAKE_C_STANDARD 23)
    set(CMAKE_C_STANDARD_REQUIRED OFF)
    set(CMAKE_C_EXTENSIONS OFF)
endif(NOT CMAKE_C_STANDARD)

if(NOT CMAKE_CXX_STANDARD)
    set(CMAKE_CXX_STANDARD 23)
    set(CMAKE_CXX_STANDARD_REQUIRED ON)
    set(CMAKE_CXX_EXTENSIONS OFF)
endif(NOT CMAKE_CXX_STANDARD)

if(TARGET ${PROJECT_NAME})
    return()
endif()

if(NOT TARGET QtExtensions)
    message(FATAL_ERROR "[Quasar Library] No Qt Extensions library found.")
endif()

if(NOT TARGET QtExtensionsNetworking)
    message(FATAL_ERROR "[Quasar Library] No Qt Extensions Networking library found.")
endif()

set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOMOC ON)

find_package(QT NAMES Qt5 Qt6 REQUIRED COMPONENTS Core)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED
  COMPONENTS
    Core
    Gui
    Network
    Positioning
)

enable_testing()
include_directories(src/c++ include)

add_library(${PROJECT_NAME} STATIC)
target_sources(${PROJECT_NAME}
  PUBLIC
    include/Quasar/Quasar
    include/Quasar/NetworkAPI
    include/Quasar/Utils
    include/Quasar/Telemetry
    include/Quasar/TelemetrySocket
    include/Quasar/Data/Status
    include/Quasar/Data/Telemetry

    resources/quasar_rc.qrc
  PRIVATE
    src/c++/quasar.h
    src/c++/quasar.c++
    src/c++/networkapi.h
    src/c++/networkapi.c++
    src/c++/utils.h
    src/c++/utils.c++
    src/c++/datagrams/telemetry.h
    src/c++/datagrams/status.h
    src/c++/types/telemetry.h
    src/c++/types/telemetry.c++
    src/c++/sockets/telemetrysocket.h
    src/c++/sockets/telemetrysocket.c++
)

target_link_libraries(${PROJECT_NAME}
  PRIVATE
    Qt${QT_VERSION_MAJOR}::Core
    Qt${QT_VERSION_MAJOR}::Gui
    Qt${QT_VERSION_MAJOR}::Network
    Qt${QT_VERSION_MAJOR}::Positioning
    QtExtensions
    QtExtensionsNetworking
)

target_include_directories(${PROJECT_NAME} PUBLIC include src/c++)