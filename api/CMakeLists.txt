﻿include_directories(include source)

file(GLOB api_src_list include/*.h source/*.cpp source/*.h source/*.c)
if (IOS)
    add_library(mk_api STATIC ${api_src_list})
    target_link_libraries(mk_api ${LINK_LIB_LIST})
else ()
    add_library(mk_api SHARED ${api_src_list})
    if (WIN32)
        add_definitions(-DMediaKitApi_EXPORTS)
    endif ()

    target_link_libraries(mk_api ${LINK_LIB_LIST})
    add_subdirectory(tests)

    #安装目录
    if (WIN32)
        set(INSTALL_PATH_LIB $ENV{HOME}/${CMAKE_PROJECT_NAME}/lib)
        set(INSTALL_PATH_INCLUDE $ENV{HOME}/${CMAKE_PROJECT_NAME}/include)
    else ()
        set(INSTALL_PATH_LIB lib)
        set(INSTALL_PATH_INCLUDE include)
    endif ()

    file(GLOB api_header_list include/*.h)
    install(FILES ${api_header_list} DESTINATION ${INSTALL_PATH_INCLUDE})
    install(TARGETS mk_api ARCHIVE DESTINATION ${INSTALL_PATH_LIB} LIBRARY DESTINATION ${INSTALL_PATH_LIB})
endif ()