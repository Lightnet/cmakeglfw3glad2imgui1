cmake_minimum_required(VERSION 3.20) # FetchContent is available in 3.11+

project(glfwimgui VERSION 0.0.1)

# figure out if color messaging is supported
#if(WIN32)
#    set(COLOR_MSG_SUPPORTED FALSE)
#else()
#    set(COLOR_MSG_SUPPORTED TRUE)
#endif()
#set(COLOR_MSG_SUPPORTED TRUE)
#message("COLOR_MSG_SUPPORTED " ${COLOR_MSG_SUPPORTED})

# https://stackoverflow.com/questions/18968979/how-to-get-colorized-output-with-cmake
# https://github.com/microsoft/vscode-cmake-tools/issues/478
# https://github.com/toeb/cmakepp/issues/75
# https://www.titanwolf.org/Network/q/ed8423c2-5b7e-47d1-a0b2-6e9e106d0d2d/y

#https://github.com/aaronmjacobs/InitGL/blob/master/CMakeLists.txt

#message("CMAKE_COMMAND: " ${CMAKE_COMMAND})

#execute_process(COMMAND ${CMAKE_COMMAND} -E cmake_echo_color --red --bold hello) #window does not work

#set(CMAKE_GENERATOR_PLATFORM Win32)
#set(CMAKE_C_FLAGS -m32)
#set(CMAKE_CXX_FLAGS -m32)

#add_compile_definitions(IMGUI_IMPL_OPENGL_ES2 1) # fail
#add_compile_definitions(IMGUI_IMPL_OPENGL_ES3 1) # fail
#add_compile_definitions(IMGUI_IMPL_OPENGL_LOADER_GL3W 1) # fail
#add_compile_definitions(IMGUI_IMPL_OPENGL_LOADER_GLEW 1) # fail
#add_compile_definitions(IMGUI_IMPL_OPENGL_LOADER_GLAD 1) #
#add_compile_definitions(IMGUI_IMPL_OPENGL_LOADER_GLAD2 1) # fail
#add_compile_definitions(IMGUI_IMPL_OPENGL_LOADER_GLBINDING2 1) # fail
#add_compile_definitions(IMGUI_IMPL_OPENGL_LOADER_GLBINDING3 1) # fail
#add_compile_definitions(IMGUI_IMPL_OPENGL_LOADER_CUSTOM 1) # fail

set(CPP_INCLUDE_DIRS "")
set(CPP_SOURCES_FILES "")
set(CPP_HEADERS_FILES "")

if (NOT glfw_FOUND)
  include(FetchContent)
  FetchContent_Declare(
    glfw
    GIT_REPOSITORY https://github.com/glfw/glfw.git
    GIT_TAG 3.3.4
  )
  FetchContent_GetProperties(glfw)
  if (NOT glfw_POPULATED) # Have we downloaded glfw yet?
    set(FETCHCONTENT_QUIET NO)
    FetchContent_Populate(glfw)

    set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE) # don't build the supplied examples
    set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE) # don't build the tests
    set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE) # don't build the docs
    set(GLFW_INSTALL OFF CACHE INTERNAL "" FORCE)
    #set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
    include_directories(${glfw_SOURCE_DIR}/include)
    list(APPEND CPP_INCLUDE_DIRS ${glfw_SOURCE_DIR}/include)

    #add_subdirectory(${glfw_SOURCE_DIR})
    message("glfw_SOURCE_DIR " ${glfw_SOURCE_DIR})
    message("glfw_BINARY_DIR " ${glfw_BINARY_DIR})
    add_subdirectory(${glfw_SOURCE_DIR} ${glfw_BINARY_DIR})
  endif()
endif()

if (NOT imgui_FOUND)
  include(FetchContent)
  FetchContent_Declare(
    imgui
    GIT_REPOSITORY https://github.com/ocornut/imgui.git
    GIT_TAG v1.82
  )
  FetchContent_GetProperties(imgui)
  if (NOT imgui_POPULATED) # Have we downloaded glfw yet?
    set(FETCHCONTENT_QUIET NO)
    FetchContent_Populate(imgui)
    #message("imgui_SOURCE_DIR " ${imgui_SOURCE_DIR})
    #include_directories(${imgui_SOURCE_DIR} ${imgui_SOURCE_DIR}/backends)
    list(APPEND CPP_SOURCES_FILES ${imgui_SOURCE_DIR}/imgui.cpp)
    list(APPEND CPP_SOURCES_FILES ${imgui_SOURCE_DIR}/imgui_demo.cpp)
    list(APPEND CPP_SOURCES_FILES ${imgui_SOURCE_DIR}/imgui_draw.cpp)
    list(APPEND CPP_SOURCES_FILES ${imgui_SOURCE_DIR}/backends/imgui_impl_glfw.cpp)
    list(APPEND CPP_SOURCES_FILES ${imgui_SOURCE_DIR}/backends/imgui_impl_opengl3.cpp)

    list(APPEND CPP_HEADERS_FILES ${imgui_SOURCE_DIR}/imgui.h)
    list(APPEND CPP_HEADERS_FILES ${imgui_SOURCE_DIR}/imconfig.h)
    list(APPEND CPP_HEADERS_FILES ${imgui_SOURCE_DIR}/imgui_internal.h)
    list(APPEND CPP_HEADERS_FILES ${imgui_SOURCE_DIR}/imstb_rectpack.h)
    list(APPEND CPP_HEADERS_FILES ${imgui_SOURCE_DIR}/imstb_textedit.h)
    list(APPEND CPP_HEADERS_FILES ${imgui_SOURCE_DIR}/imstb_truetype.h)
    
    list(APPEND CPP_HEADERS_FILES ${imgui_SOURCE_DIR}/backends/imgui_impl_glfw.h)
    list(APPEND CPP_HEADERS_FILES ${imgui_SOURCE_DIR}/backends/imgui_impl_opengl3.h)
    list(APPEND CPP_INCLUDE_DIRS ${imgui_SOURCE_DIR})
    list(APPEND CPP_INCLUDE_DIRS ${imgui_SOURCE_DIR}/backends)
    #message("imgui_BINARY_DIR " ${imgui_BINARY_DIR})
    #add_subdirectory(${glfw_SOURCE_DIR} ${glfw_BINARY_DIR})
  endif()
endif()

message("PROJECT_SOURCE_DIR " ${PROJECT_SOURCE_DIR})
message("CPP_INCLUDE_DIRS " ${CPP_INCLUDE_DIRS})
#include_directories(${PROJECT_SOURCE_DIR}/include)
include_directories(${CPP_INCLUDE_DIRS})

set(GLAD_DIR "${PROJECT_SOURCE_DIR}/glad")

#look for lib file
find_library(glfw3_LIB NAMES glfw3 HINTS ${glfw_BINARY_DIR}/src/Debug)
message("glfw3_LIB " ${glfw3_LIB})
#================================================
#add_executable(${PROJECT_NAME} main.cpp)
add_executable(${PROJECT_NAME} 
  #glad1glfw3.cpp
  #glad2glfw3.cpp
  glfw_opengl3_imgui.cpp 
  #${GLAD_DIR} 
  #${CPP_SOURCES_FILES}
  #${CPP_HEADERS_FILES}
  #${imgui_SOURCE_DIR}/imconfig.h
  ${imgui_SOURCE_DIR}/imgui.cpp 
  ${imgui_SOURCE_DIR}/imgui.h
  ${imgui_SOURCE_DIR}/imgui_demo.cpp
  ${imgui_SOURCE_DIR}/imgui_draw.cpp
  #${imgui_SOURCE_DIR}/imgui_internal.h
  ${imgui_SOURCE_DIR}/imgui_tables.cpp
  ${imgui_SOURCE_DIR}/imgui_widgets.cpp
  #${imgui_SOURCE_DIR}/imstb_rectpack.h
  #${imgui_SOURCE_DIR}/imstb_textedit.h
  #${imgui_SOURCE_DIR}/imstb_truetype.h
  ${imgui_SOURCE_DIR}/backends/imgui_impl_glfw.h
  ${imgui_SOURCE_DIR}/backends/imgui_impl_glfw.cpp
  ${imgui_SOURCE_DIR}/backends/imgui_impl_opengl3.cpp
  ${imgui_SOURCE_DIR}/backends/imgui_impl_opengl3.h
)
#target_include_directories(${PROJECT_NAME} )
#target_link_libraries(${PROJECT_NAME} PRIVATE glfw3)

# GLAD 1.0
set(GLAD_DIR "${PROJECT_SOURCE_DIR}/glad")
add_library(glad ${GLAD_DIR}/src/gl.c)
target_include_directories(glad PRIVATE ${GLAD_DIR}/include)
target_include_directories(${PROJECT_NAME} PRIVATE ${GLAD_DIR}/include)
#target_link_libraries(${PROJECT_NAME} PUBLIC glad ${CMAKE_DL_LIBS} PRIVATE ${glfw3_LIB}) #pass
target_link_libraries(${PROJECT_NAME} PRIVATE ${glfw3_LIB} glad ${CMAKE_DL_LIBS})
