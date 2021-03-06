find_package (PkgConfig)

# Add all your dependencies to the list below
pkg_check_modules (DEPS REQUIRED gthread-2.0 gtk+-3.0 wingpanel-2.0 granite libgtop-2.0 libbamf3 libudev)

add_definitions (${DEPS_CFLAGS})
link_directories (${DEPS_LIBRARY_DIRS})

find_package (Vala REQUIRED)
include (ValaVersion)
ensure_vala_version ("0.22.0" MINIMUM)

include (ValaPrecompile)
# Add all your vala files and requires packages to the List below to include them in the build
vala_precompile (VALA_C ${CMAKE_PROJECT_NAME}
  Indicator.vala
  Utils.vala
  Widgets/DisplayWidget.vala
  Widgets/PopoverWidget.vala
  Widgets/AppList.vala
  Widgets/DeviceList.vala
  Widgets/ScreenBrightness.vala
  Services/SettingsManager.vala
  Services/DeviceManager.vala
  Services/Device.vala
  Services/AppManager.vala
  Services/DBusInterfaces/Properties.vala
  Services/DBusInterfaces/UPower.vala
  Services/DBusInterfaces/Device.vala
  Services/DBusInterfaces/Screen.vala
  Services/ProcessMonitor/Monitor.vala
  Services/ProcessMonitor/Process.vala
  Services/Backlight/Backlight.vala
  ${CMAKE_CURRENT_BINARY_DIR}/config.vala
PACKAGES
  wingpanel-2.0
  granite
  libbamf3
  posix
CUSTOM_VAPIS
  ../vapi/libgtop-2.0.vapi
  ../vapi/libudev.vapi
OPTIONS
  --thread
)

add_library (${CMAKE_PROJECT_NAME} MODULE ${VALA_C})
target_link_libraries(${CMAKE_PROJECT_NAME} ${DEPS_LIBRARIES})

# Installation
install (TARGETS ${CMAKE_PROJECT_NAME} DESTINATION ${PKGDATADIR})
