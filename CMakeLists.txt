cmake_minimum_required(VERSION 3.14)
project(imgui_test)

set(CMAKE_CXX_STANDARD 17)

find_package(OpenGL REQUIRED)
find_package(OpenCL REQUIRED)

if(APPLE)
    set(GLEW_INCLUDE_DIRS /usr/local/Cellar/glew/2.2.0_1/include/GL)
    set(GLEW_LIBRARIES /usr/local/Cellar/glew/2.2.0_1/lib/libGLEW.2.2.0.dylib)
else()
    find_package(GLEW REQUIRED)
endif()

include_directories(${GLEW_INCLUDE_DIRS})

include(FetchContent)
FetchContent_Declare(
        glfw
        GIT_REPOSITORY https://github.com/glfw/glfw.git
        GIT_TAG 3.3.8
)
FetchContent_MakeAvailable(glfw)

# ImGui directories
set(IMGUI_DIR imgui)
set(GLFW_DIR ${CMAKE_BINARY_DIR}/glfw)
set(IMGUI_IMPL_DIR imgui/backends)
# Add ImGui source files
set(IMGUI_SRC
        ${IMGUI_DIR}/imgui.cpp
        ${IMGUI_DIR}/imgui_draw.cpp
        ${IMGUI_DIR}/imgui_tables.cpp
        ${IMGUI_DIR}/imgui_widgets.cpp
        ${IMGUI_IMPL_DIR}/imgui_impl_glfw.cpp
        ${IMGUI_IMPL_DIR}/imgui_impl_opengl3.cpp
        )

add_executable(${PROJECT_NAME} main.cpp ${IMGUI_SRC})
target_include_directories(${PROJECT_NAME} PUBLIC ${IMGUI_DIR} ${GLFW_DIR} ${IMGUI_IMPL_DIR})

target_link_libraries(${PROJECT_NAME} PRIVATE glfw OpenGL::GL ${GLEW_LIBRARIES} OpenCL::OpenCL)
