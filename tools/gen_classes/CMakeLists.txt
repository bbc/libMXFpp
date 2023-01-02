add_executable(gen_classes
    gen_metadata_classes.cpp
)
target_link_libraries(gen_classes PRIVATE
    ${MXF_link_lib}
)


add_custom_target(libMXFpp_generate_classes
    COMMAND $<TARGET_FILE:gen_classes> ${CMAKE_BINARY_DIR}/generated_classes
)
