# Build a static library for the other examples
add_library(libmxfpp_examples_common STATIC
    DynamicByteArray.cpp
)

set(libmxfpp_examples_common_headers
    CommonTypes.h
    DynamicByteArray.h
)

target_include_directories(libmxfpp_examples_common PUBLIC
    ${PROJECT_SOURCE_DIR}/examples/Common
)

target_include_directories(libmxfpp_examples_common PRIVATE
    ${PROJECT_BINARY_DIR}
)
target_compile_definitions(libmxfpp_examples_common PRIVATE
    HAVE_CONFIG_H=1
)

target_link_libraries(libmxfpp_examples_common PUBLIC
    MXFpp
    ${MXF_link_lib}
)

if(UNIX)
    # The static library will be linked into the dynamic libraries of the examples.
    # The -fPIC compile option ensures it generates position independent code
    target_compile_options(libmxfpp_examples_common PRIVATE
        -fPIC
    )
endif()

install(FILES ${libmxfpp_examples_common_headers} DESTINATION include/libMXF++/examples/Common)
