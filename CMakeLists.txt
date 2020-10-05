cmake_minimum_required(VERSION 3.16)
project(app)

#sudo apt-get install libgrpc-dev libgrpc++-dev protobuf-compiler-grpc libprotobuf-dev
find_package(PkgConfig REQUIRED)
pkg_search_module(GRPC REQUIRED IMPORTED_TARGET  grpc)
pkg_search_module(GRPCPP REQUIRED IMPORTED_TARGET  grpc++)

find_package(Protobuf REQUIRED)

#find_package(gRPC CONFIG REQUIRED)

#find_package(gRPC REQUIRED PATHS "/mnt/c/Users/wiecz/Desktop/gcloud_playground/grpc/cmake/build")


#file(GLOB ProtoFiles "${CMAKE_CURRENT_SOURCE_DIR}/*.proto")
#message("${_gRPC_PROTOBUF_PROTOC_EXECUTABLE}")
#message("${ProtoFiles}")
#PROTOBUF_GENERATE_GRPC_CPP(${ProtoFiles})
#PROTOBUF_GENERATE_GRPC_CPP(ProtoGRPCSources ProtoGRPCHeaders ${ProtoFiles})
#PROTOBUF_GENERATE_GRPC_CPP("ProcessText.proto")
#message("${ProtoGRPCSources} AND ${ProtoGRPCHeaders}")

add_subdirectory(schemas)


file(GLOB sources "server.cpp")
add_executable(${PROJECT_NAME}_server ${sources})
#set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS}  -Wl,--whole-archive")
target_link_libraries(${PROJECT_NAME}_server PkgConfig::GRPC)
target_link_libraries(${PROJECT_NAME}_server PkgConfig::GRPCPP)

target_link_libraries(${PROJECT_NAME}_server schemas)
