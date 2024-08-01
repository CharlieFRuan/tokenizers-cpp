
# Example cmake project
cmake_minimum_required(VERSION 3.18)
project(tokenizers_cpp_example C CXX)

include(CheckCXXCompilerFlag)
if(NOT MSVC)
  check_cxx_compiler_flag("-std=c++17" SUPPORT_CXX17)
  set(CMAKE_CXX_FLAGS "-std=c++17 ${CMAKE_CXX_FLAGS}")
  set(CMAKE_CUDA_STANDARD 17)
else()
  check_cxx_compiler_flag("/std:c++17" SUPPORT_CXX17)
  set(CMAKE_CXX_FLAGS "/std:c++17 ${CMAKE_CXX_FLAGS}")
  set(CMAKE_CUDA_STANDARD 17)
endif()

# set(CMAKE_EXE_LINKER_FLAGS ${CMAKE_EXE_LINKER_FLAGS} "-flto")

# include tokenizer cpp as a sub directory
set(TOKENZIER_CPP_PATH ..)
add_subdirectory(${TOKENZIER_CPP_PATH} tokenizers EXCLUDE_FROM_ALL)

# add_executable(example example.cc example.h)
add_executable(example example.cc)

# set(CMAKE_EXECUTABLE_SUFFIX ".wasm")

# add_library(example SHARED example.cc)

target_include_directories(example PRIVATE ${TOKENZIER_CPP_PATH}/include)

# You can link tokenizers_cpp, it will automatically link tokenizers_c
# and sentencepiece libary
# target_link_libraries(example PRIVATE tokenizers_cpp -Wl,--whole-archive,-u,htonl,-u,htons)
target_link_libraries(example PRIVATE tokenizers_cpp -Wl,--whole-archive)
# target_link_libraries(example PRIVATE tokenizers_cpp)

# set_target_properties(example PROPERTIES LINK_FLAGS "-s WASM_ASYNC_COMPILATION=0 -s EXIT_RUNTIME=1  --bind -lnodefs.js")
set_target_properties(example PROPERTIES LINK_FLAGS "-s STANDALONE_WASM --no-entry --bind -lnodefs.js")
# set_target_properties(example PROPERTIES LINK_FLAGS "--bind -lnodefs.js")
# target_compile_options(example PUBLIC ${CMAKE_C_FLAGS} -emit-llvm)

# set_target_properties(example PROPERTIES COMPILE_FLAGS "-Os -s SIDE_MODULE=1 ")
# set_target_properties(example PROPERTIES LINK_FLAGS    "-Os -s WASM=1 -s SIDE_MODULE=1 -s STANDALONE_WASM --no-entry --bind -lnodefs.js")
