# Copyright (C) 2007-2010, Evgeny Ratnikov
#
# This file is part of termit.
# termit is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License version 2 
# as published by the Free Software Foundation.
# termit is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
# You should have received a copy of the GNU General Public License
# along with termit. If not, see <http://www.gnu.org/licenses/>.

PROJECT(TERMIT C)

cmake_minimum_required(VERSION 2.6.1 FATAL_ERROR)
SET(CMAKE_VERSION "${CMAKE_CACHE_MAJOR_VERSION}.${CMAKE_CACHE_MINOR_VERSION}.${CMAKE_CACHE_RELEASE_VERSION}")

INCLUDE(UsePkgConfig)

ADD_DEFINITIONS(-Wall)

SET(DEFAULT_XDG_DIR "/etc/xdg")
string(REPLACE ":" ";" XDG_DIRS "$ENV{XDG_CONFIG_DIRS}")
FOREACH(xdgDir ${XDG_DIRS})
  SET(XDG_DIR ${xdgDir})
  BREAK()
ENDFOREACH()

IF(NOT XDG_DIR)
  SET(XDG_DIR "/etc/xdg")
ENDIF(NOT XDG_DIR)

SET(DEBUG FALSE CACHE BOOL "debug info")
SET(ENABLE_NLS TRUE CACHE BOOL "enable translation")
SET(XDG_DIR ${XDG_DIR} CACHE STRING "default xdg dir")

SET(TERMIT_PACKAGE "termit")
SET(TERMIT_PACKAGE_VERSION "2.9.2")
SET(TERMIT_LOCALE_DIR "${CMAKE_INSTALL_PREFIX}/share/locale")
SET(TERMIT_XDG_DIR "${XDG_DIR}/termit")
SET(TERMIT_PACKAGE_NAME "termit - terminal emulator")
SET(TERMIT_PACKAGE_STRING "${TERMIT_PACKAGE} ${TERMIT_PACKAGE_VERSION}")
SET(TERMIT_PACKAGE_BUGREPORT "ratnikov.ev@gmail.com")

CONFIGURE_FILE(${TERMIT_SOURCE_DIR}/config.h.in ${TERMIT_SOURCE_DIR}/config.h)

CONFIGURE_FILE(
  "${CMAKE_CURRENT_SOURCE_DIR}/cmake/cmake_uninstall.cmake.in"
  "${CMAKE_CURRENT_BINARY_DIR}/cmake/cmake_uninstall.cmake"
  IMMEDIATE @ONLY)
ADD_CUSTOM_TARGET(uninstall
  "${CMAKE_COMMAND}" -P "${CMAKE_CURRENT_BINARY_DIR}/cmake/cmake_uninstall.cmake")

ADD_SUBDIRECTORY(doc)
ADD_SUBDIRECTORY(src)
IF(ENABLE_NLS)
  ADD_SUBDIRECTORY(po)
ENDIF(ENABLE_NLS)
ADD_SUBDIRECTORY(utils)
ADD_SUBDIRECTORY(etc)

MESSAGE("\n  termit ${TERMIT_PACKAGE_VERSION}\n")
MESSAGE("Prefix dir:.......... ${CMAKE_INSTALL_PREFIX}")
IF(ENABLE_NLS)
  MESSAGE("Locales dir:......... ${TERMIT_LOCALE_DIR}")
ENDIF(ENABLE_NLS)
MESSAGE("Xdg dir:............. ${TERMIT_XDG_DIR}")
MESSAGE("Debug:............... ${DEBUG}")
MESSAGE("\n")

