set(warnings -Wall -Wextra -Wno-unused-parameter)

add_compile_options(
  "$<$<CXX_COMPILER_ID:Clang>:${warnings}>"
  "$<$<CXX_COMPILER_ID:AppleClang>:${warnings}>"
  "$<$<CXX_COMPILER_ID:GNU>:${warnings}>"
)

# =============================================================================
# autocheck tests

add_executable(oscpp_autocheck oscpp_autocheck.cpp)

target_include_directories(oscpp_autocheck PRIVATE autocheck/include)
target_link_libraries(oscpp_autocheck PRIVATE ${OSCPP_TARGET})

add_test(oscpp_autocheck oscpp_autocheck)

# =============================================================================
# README code

add_custom_command(
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/README.cpp
    COMMAND ruby ${CMAKE_CURRENT_SOURCE_DIR}/../tools/mdcode.rb
                 ${CMAKE_CURRENT_SOURCE_DIR}/../README.md
                 ${CMAKE_CURRENT_BINARY_DIR}/README.cpp
    MAIN_DEPENDENCY ${CMAKE_CURRENT_SOURCE_DIR}/../README.md
)

add_executable(oscpp_readme ${CMAKE_CURRENT_BINARY_DIR}/README.cpp)

target_link_libraries(oscpp_readme PRIVATE ${OSCPP_TARGET})

add_test(oscpp_readme oscpp_readme)
