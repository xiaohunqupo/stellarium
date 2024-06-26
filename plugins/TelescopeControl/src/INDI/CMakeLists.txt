set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)

find_package(Threads REQUIRED)

SET(TelescopeControl_INDI_UIS
    TelescopeClientINDIWidget.ui
    INDIControlWidget.ui
    )

IF (${QT_VERSION_MAJOR} EQUAL "5")
     QT5_WRAP_UI(TelescopeControl_INDI_UIS_H ${TelescopeControl_INDI_UIS})
ELSE()
    QT_WRAP_UI(TelescopeControl_INDI_UIS_H ${TelescopeControl_INDI_UIS})
ENDIF()

add_library(TelescopeControl_INDI STATIC
    INDIConnection.hpp
    INDIConnection.cpp
    TelescopeClientINDI.hpp
    TelescopeClientINDI.cpp
    TelescopeClientINDIWidget.hpp
    TelescopeClientINDIWidget.cpp
    INDIControlWidget.hpp
    INDIControlWidget.cpp
    ${TelescopeControl_INDI_UIS_H}
    )

target_link_libraries(TelescopeControl_INDI
    Qt${QT_VERSION_MAJOR}::Core
    Qt${QT_VERSION_MAJOR}::Network
    indiclient
    TelescopeControl_common
    ${CMAKE_THREAD_LIBS_INIT}
    )

target_include_directories(TelescopeControl_INDI INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})

IF(ENABLE_TESTING)
    add_subdirectory(test)
ENDIF(ENABLE_TESTING)

SET_TARGET_PROPERTIES(TelescopeControl_INDI PROPERTIES FOLDER "plugins/TelescopeControl")
