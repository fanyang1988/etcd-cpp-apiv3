cmake_minimum_required (VERSION 3.1.3 FATAL_ERROR)
set(PROJECT_NAME "etcd-cpp-api")
project (${PROJECT_NAME})

set(CMAKE_CXX_STANDARD 11)

set (etcd-cpp-api_VERSION_MAJOR 0)
set (etcd-cpp-api_VERSION_MINOR 1)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

# use vspkg
#
#
#
#

# CMake projects should use: 
# "-DCMAKE_TOOLCHAIN_FILE=E:/vcpkg-master/scripts/buildsystems/vcpkg.cmake"
# or set CMAKE_TOOLCHAIN_FILE by "Add Entry" in cmake-gui

if (WIN32)
    set(Boost_USE_STATIC_LIBS        ON)  # only find static libs
    set(Boost_USE_DEBUG_LIBS         OFF) # ignore debug libs and
    set(Boost_USE_RELEASE_LIBS       ON)  # only find release libs
    set(Boost_USE_MULTITHREADED      ON)
    set(Boost_USE_STATIC_RUNTIME    OFF)


    add_library(etcd-cpp-api 
        ./proto/kv.pb.cc 
        ./proto/auth.pb.cc 
        ./proto/rpc.pb.cc 
        ./proto/rpc.grpc.pb.cc 
        ./v3/src/AsyncTxnResponse.cpp 
        ./v3/src/AsyncRangeResponse.cpp 
        ./v3/src/Transaction.cpp 
        ./v3/src/action_constants.cpp 
        ./v3/src/AsyncSetAction.cpp 
        ./v3/src/AsyncCompareAndSwapAction.cpp 
        ./v3/src/AsyncUpdateAction.cpp 
        ./v3/src/AsyncGetAction.cpp 
        ./v3/src/AsyncDeleteAction.cpp 
        ./v3/src/AsyncCompareAndDeleteAction.cpp 
        ./v3/src/Action.cpp 
        ./v3/src/AsyncWatchAction.cpp 
        ./v3/src/V3Response.cpp 
        ./v3/src/AsyncDeleteRangeResponse.cpp 
        ./v3/src/AsyncWatchResponse.cpp 
        ./v3/src/AsyncLeaseGrantResponse.cpp 
        ./v3/src/AsyncLeaseGrantAction.cpp 
        ./v3/src/KeyValue.cpp
        ./src/Client.cpp
        ./src/Response.cpp
        ./src/Value.cpp
        ./src/SyncClient.cpp
        ./src/Watcher.cpp )

    # use vspkg
    find_package(cpprestsdk REQUIRED)
    # Note: 1 targets were omitted
    #target_link_libraries(${PROJECT_NAME} PRIVATE 
    #    cpprestsdk::cpprest)

    find_package(Boost REQUIRED COMPONENTS system)
    include_directories(SYSTEM ${Boost_INCLUDE_DIR})

    find_package(grpc REQUIRED)
    # Note: 6 targets were omitted
    #target_link_libraries(etcd-cpp-api PRIVATE 
    #                gRPC::gpr 
    #                gRPC::grpc 
    #                gRPC::grpc_cronet 
    #                gRPC::grpc_unsecure)

    find_package(protobuf REQUIRED)
    #target_link_libraries(etcd-cpp-api PRIVATE 
    #                protobuf::libprotobuf-lite 
    #                protobuf::libprotobuf 
    #                protobuf::libprotoc)

    add_definitions(-D_WIN32_WINNT=0x600)

else (WIN32)
    find_library(CPPREST_LIB NAMES cpprest)
    find_path(CPPREST_INCLUDE_DIR NAMES cpprest/http_client.h)
    find_package(Boost REQUIRED COMPONENTS system thread locale random)
    include_directories(SYSTEM ${CPPREST_INCLUDE_DIR} ${Boost_INCLUDE_DIR})
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wpedantic -Werror -std=c++11")
endif (WIN32)
