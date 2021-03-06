set(APP_NAME hellocpp)

if(QT)
  set(PLATFORM_SRC
    proj.qt/main.cpp
  )
elseif(ANDROID)
  set(PLATFORM_SRC
    proj.android/jni/hellocpp/main.cpp
  )
elseif(WIN32)
  set(PLATFORM_SRC
    proj.win32/main.cpp
  )
elseif(APPLE)
  if(IOS)
    set(PLATFORM_SRC
      proj.ios/main.m
	  proj.ios/AppController.mm
	  proj.ios/RootViewController.mm
    )
  else()
    set(PLATFORM_SRC
      proj.mac/main.m
    )
  endif()
else()
  set(PLATFORM_SRC
    proj.linux/main.cpp
  )
  endif()

set(SAMPLE_SRC
  ${PLATFORM_SRC}
  Classes/AppDelegate.cpp
  Classes/HelloWorldScene.cpp
)

# add the executable
add_executable(${APP_NAME}
  ${SAMPLE_SRC}
)

#get our resources
add_custom_command(TARGET ${APP_NAME} PRE_BUILD
                   COMMAND ${CMAKE_COMMAND} -E copy_directory
                       ${CMAKE_CURRENT_SOURCE_DIR}/Resources ${CMAKE_CURRENT_BINARY_DIR})

if(WIN32 AND MSVC)

  #get our dlls
  add_custom_command(TARGET ${APP_NAME} PRE_BUILD
                     COMMAND ${CMAKE_COMMAND} -E copy
                     ${CMAKE_CURRENT_SOURCE_DIR}/../../../external/win32-specific/gles/prebuilt/glew32.dll 
					 ${CMAKE_CURRENT_BINARY_DIR})

  add_custom_command(TARGET ${APP_NAME} PRE_BUILD
                     COMMAND ${CMAKE_COMMAND} -E copy
                     ${CMAKE_CURRENT_SOURCE_DIR}/../../../external/win32-specific/zlib/prebuilt/zlib1.dll 
					 ${CMAKE_CURRENT_BINARY_DIR}/Debug)

  #Visual Studio Defaults to wrong type
  set_target_properties(${APP_NAME} PROPERTIES LINK_FLAGS_DEBUG "/SUBSYSTEM:WINDOWS")
  set_target_properties(${APP_NAME} PROPERTIES LINK_FLAGS_RELEASE "/SUBSYSTEM:WINDOWS")
endif()

target_link_libraries(${APP_NAME} audio cocos2d)
