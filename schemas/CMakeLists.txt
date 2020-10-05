project(schemas)

file(GLOB protos "*.proto")

set(CodeGen ${CMAKE_CURRENT_BINARY_DIR}/CodeGen)
file(MAKE_DIRECTORY ${CodeGen})
set(sources)

foreach(in_file ${protos})
        string(REGEX REPLACE "[.]proto$" ".grpc.pb.cc" in_file_grpc ${in_file})
        string(REGEX REPLACE "[.]proto$" ".pb.cc" in_file_cpp ${in_file})
        get_filename_component(in_filename_grpc ${in_file_grpc} NAME)
        get_filename_component(in_filename_cpp ${in_file_cpp} NAME)

	add_custom_command(
            OUTPUT ${CodeGen}/${in_filename_cpp} ${CodeGen}/${in_filename_grpc}
            COMMAND protoc -I=${CMAKE_CURRENT_SOURCE_DIR} --grpc_out=${CodeGen}  --plugin=protoc-gen-grpc=`which grpc_cpp_plugin` ${in_file}
            COMMAND protoc -I=${CMAKE_CURRENT_SOURCE_DIR} --cpp_out=${CodeGen} ${in_file}
            DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/${schemas}
	)
    list(APPEND sources ${CodeGen}/${in_filename_cpp} ${CodeGen}/${in_filename_grpc})
endforeach()

add_library(${PROJECT_NAME} OBJECT ${sources})
target_include_directories(${PROJECT_NAME} PUBLIC ${CodeGen})
target_link_libraries(${PROJECT_NAME} ${Protobuf_LIBRARIES})
target_link_libraries(${PROJECT_NAME} PkgConfig::GRPC)
target_link_libraries(${PROJECT_NAME} PkgConfig::GRPCPP)

