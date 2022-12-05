cmake_minimum_required(VERSION 3.12.4)

project(DDSHelloWorld)

# Find requirements
find_package(fastcdr REQUIRED)
find_package(fastrtps REQUIRED)

# Set C++11
add_compile_options(-std=c++11)

# include
include_directories("include/idl_generate/")

# datatype 
file(GLOB DDS_HELLOWORLD_SOURCES_CXX "./include/idl_generate/*.cxx")
add_library(HelloWorld_IDL_lib ${DDS_HELLOWORLD_SOURCES_CXX})
# target_link_libraries(HelloWorld_IDL_lib 
#     fastrtps 
#     fastcdr
# )

# Publisher
add_executable(DDSHelloWorldPublisher src/HelloWorldPublisher.cpp)
target_link_libraries(DDSHelloWorldPublisher 
    HelloWorld_IDL_lib
    fastrtps 
    fastcdr
)

# Subscriber
add_executable(DDSHelloWorldSubscriber src/HelloWorldSubscriber.cpp)
target_link_libraries(DDSHelloWorldSubscriber     
    HelloWorld_IDL_lib    
    fastrtps 
    fastcdr
)