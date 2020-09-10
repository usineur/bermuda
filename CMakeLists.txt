cmake_minimum_required(VERSION 2.8)

# VitaSDK defines
if( NOT DEFINED CMAKE_TOOLCHAIN_FILE )
  if( DEFINED ENV{VITASDK} )
    set(CMAKE_TOOLCHAIN_FILE "$ENV{VITASDK}/share/vita.toolchain.cmake" CACHE PATH "toolchain file")
  else()
    message(FATAL_ERROR "Please define VITASDK to point to your SDK path!")
  endif()
endif()

# Project start
set(VITA_APPNAME bermuda-vita)
set(VITA_TITLEID "BERM00001")
set(VITA_VERSION "01.00")

project(${VITA_APPNAME})
include("${VITASDK}/share/vita.cmake" REQUIRED)

# Flags and includes
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -g -DBERMUDA_VORBIS -DBERMUDA_ZLIB -fno-exceptions")

include_directories(
  ${VITASDK}/arm-vita-eabi/include/SDL2
)

link_directories(
  ${CMAKE_CURRENT_BINARY_DIR}
)

# Builds
add_executable(${VITA_APPNAME}
  avi_player.cpp
  bag.cpp
  decoder.cpp
  dialogue.cpp
  file.cpp
  fs.cpp
  game.cpp
  main.cpp
  menu.cpp
  mixer_sdl.cpp
  mixer_soft.cpp
  opcodes.cpp
  parser_dlg.cpp
  parser_scn.cpp
  random.cpp
  resource.cpp
  saveload.cpp
  screenshot.cpp
  staticres.cpp
  str.cpp
  systemstub_sdl.cpp
  util.cpp
  win16.cpp
)

target_link_libraries(${VITA_APPNAME}
  SDL2_mixer
  SDL2
  vita2d
  vorbisfile
  vorbis
  ogg
  mpg123
  mikmod
  modplug
  FLAC
  z
  SceAppMgr_stub
  SceAudio_stub
  SceCommonDialog_stub
  SceCtrl_stub
  SceDisplay_stub
  SceGxm_stub
  SceHid_stub
  ScePower_stub
  SceSysmodule_stub
  SceTouch_stub
)

# Create Vita artifacts
vita_create_self(eboot.bin ${VITA_APPNAME} UNSAFE)
vita_create_vpk(${VITA_APPNAME}.vpk ${VITA_TITLEID} eboot.bin
  VERSION ${VITA_VERSION}
  NAME ${VITA_APPNAME}
  FILE res/icon0.png sce_sys/icon0.png
  FILE res/bg.png sce_sys/livearea/contents/bg.png
  FILE res/startup.png sce_sys/livearea/contents/startup.png
  FILE res/template.xml sce_sys/livearea/contents/template.xml
  FILE res/track01.ogg MUSIC/track01.ogg
  FILE res/track02.ogg MUSIC/track02.ogg
  FILE res/track03.ogg MUSIC/track03.ogg
  FILE res/track04.ogg MUSIC/track04.ogg
  FILE res/track05.ogg MUSIC/track05.ogg
  FILE res/track06.ogg MUSIC/track06.ogg
  FILE res/track07.ogg MUSIC/track07.ogg
  FILE res/track08.ogg MUSIC/track08.ogg
  FILE res/track09.ogg MUSIC/track09.ogg
  FILE res/track10.ogg MUSIC/track10.ogg
  FILE res/track11.ogg MUSIC/track11.ogg
  FILE res/track12.ogg MUSIC/track12.ogg
)
