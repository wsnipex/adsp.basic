project(libsamplerate)

cmake_minimum_required(VERSION 2.8)

file(WRITE config-patch.cmake
    "if(EXISTS config.sub AND EXISTS config.guess)\n
      file(COPY config.sub config.guess DESTINATION Cfg/)\n
     endif()")


include(ExternalProject)
externalproject_add(libsamplerate
                    SOURCE_DIR ${CMAKE_SOURCE_DIR}
                    PATCH_COMMAND ${CMAKE_COMMAND} -P config-patch.cmake
                    UPDATE_COMMAND ""
                    CONFIGURE_COMMAND <SOURCE_DIR>/configure 
                      --prefix=${OUTPUT_DIR}
                      --enable-static
                      --disable-shared
                      "CFLAGS=${CMAKE_C_FLAGS}"
                    INSTALL_COMMAND ""
                    BUILD_IN_SOURCE 1)

install(CODE "execute_process(COMMAND make install WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})")
