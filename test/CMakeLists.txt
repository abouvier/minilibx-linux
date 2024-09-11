add_executable(mlx-test main.c)

target_link_libraries(mlx-test PRIVATE 42Paris::MiniLibX)

add_test(
  NAME mlx-test
  COMMAND mlx-test
  WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}")
