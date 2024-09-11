cmake_minimum_required(VERSION 3.23)

project(
  MiniLibX
  VERSION 0.1
  DESCRIPTION "Simple X-Window Interface Library for students"
  HOMEPAGE_URL "https://github.com/42Paris/minilibx-linux"
  LANGUAGES C)

find_package(X11 REQUIRED)

add_library(
  mlx
  mlx_init.c
  mlx_new_window.c
  mlx_pixel_put.c
  mlx_loop.c
  mlx_mouse_hook.c
  mlx_key_hook.c
  mlx_expose_hook.c
  mlx_loop_hook.c
  mlx_int_anti_resize_win.c
  mlx_int_do_nothing.c
  mlx_int_wait_first_expose.c
  mlx_int_get_visual.c
  mlx_flush_event.c
  mlx_string_put.c
  mlx_set_font.c
  mlx_new_image.c
  mlx_get_data_addr.c
  mlx_put_image_to_window.c
  mlx_get_color_value.c
  mlx_clear_window.c
  mlx_xpm.c
  mlx_int_str_to_wordtab.c
  mlx_destroy_window.c
  mlx_int_param_event.c
  mlx_int_set_win_event_mask.c
  mlx_hook.c
  mlx_rgb.c
  mlx_destroy_image.c
  mlx_mouse.c
  mlx_screen_size.c
  mlx_destroy_display.c)

set_target_properties(mlx PROPERTIES EXPORT_NAME ${PROJECT_NAME})

target_link_libraries(mlx PRIVATE X11::Xext X11::Xkb X11::Xutil)

target_sources(mlx PUBLIC FILE_SET HEADERS FILES mlx.h)

include(CMakePackageConfigHelpers)
include(GNUInstallDirs)

configure_file(mlx.pc.in mlx.pc @ONLY)

configure_package_config_file(
  ${PROJECT_NAME}Config.cmake.in ${PROJECT_NAME}Config.cmake
  INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME})

install(
  TARGETS mlx
  EXPORT ${PROJECT_NAME}Targets
  FILE_SET HEADERS
  INCLUDES
  DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

install(
  EXPORT ${PROJECT_NAME}Targets
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
  NAMESPACE 42Paris::)

install(FILES "${CMAKE_CURRENT_BINARY_DIR}/mlx.pc"
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig)

install(FILES "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME})

install(DIRECTORY man/ TYPE MAN)

add_library(42Paris::${PROJECT_NAME} ALIAS mlx)

include(CTest)

if(BUILD_TESTING)
  add_subdirectory(test)
endif()
