include_directories (${CMAKE_CURRENT_SOURCE_DIR})

set (LIBRF24_LIBS 
    rf24-bcm
)

# add the target
add_executable(rpi-hub rpi-hub.cpp)
target_link_libraries(rpi-hub ${LIBRF24_LIBS})
install(TARGETS rpi-hub DESTINATION /usr/bin)

add_executable(scanner scanner.cpp)
target_link_libraries(scanner ${LIBRF24_LIBS})
install(TARGETS scanner DESTINATION /usr/bin)
