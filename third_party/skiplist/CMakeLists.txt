#if(POLICY CMP0063)
#    cmake_policy(SET CMP0063 NEW)
#endif()

add_library(skiplistlib STATIC SkipList.cpp)

target_include_directories(
  skiplistlib
  PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>)
set_target_properties(skiplistlib PROPERTIES EXPORT_NAME skiplistlib)

install(TARGETS skiplistlib
        EXPORT "${HIQE_EXPORT_SET}"
        LIBRARY DESTINATION "${INSTALL_LIB_DIR}"
        ARCHIVE DESTINATION "${INSTALL_LIB_DIR}")

disable_target_warnings(skiplistlib)
