#if(POLICY CMP0063)
#    cmake_policy(SET CMP0063 NEW)
#endif()

add_library(yyjson STATIC yyjson.cpp)

target_include_directories(
        yyjson
        PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>)
set_target_properties(yyjson PROPERTIES EXPORT_NAME hiqe_yyjson)

install(TARGETS yyjson
        EXPORT "${HIQE_EXPORT_SET}"
        LIBRARY DESTINATION "${INSTALL_LIB_DIR}"
        ARCHIVE DESTINATION "${INSTALL_LIB_DIR}")

#disable_target_warnings(duckdb_yyjson)
