include_directories (${CMAKE_CURRENT_SOURCE_DIR})

set (LIBRF24_LIBS 
    rf24-bcm
)

# Include extra examples 
if (BUILD_EXAMPLES_EXTRA)
   add_subdirectory (extra)
endif() 

# add the target
add_executable(gettingstarted gettingstarted.cpp)
target_link_libraries(gettingstarted ${LIBRF24_LIBS})
install(TARGETS gettingstarted DESTINATION /usr/bin)

add_executable(gettingstarted_call_response gettingstarted_call_response.cpp)
target_link_libraries(gettingstarted_call_response ${LIBRF24_LIBS})
install(TARGETS gettingstarted_call_response DESTINATION /usr/bin)

add_executable(pingpair_dyn pingpair_dyn.cpp)
target_link_libraries(pingpair_dyn ${LIBRF24_LIBS})
install(TARGETS pingpair_dyn DESTINATION /usr/bin)

add_executable(transfer transfer.cpp)
target_link_libraries(transfer ${LIBRF24_LIBS})
install(TARGETS transfer DESTINATION /usr/bin)

