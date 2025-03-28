cmake_minimum_required(VERSION 3.14...3.28)

# Project name
project(FileWatch 
        VERSION 0.0.1 
        DESCRIPTION "A C++ File Watcher Library"
        LANGUAGES CXX)

# Build options
option(BUILD_TESTS "Build the unit tests" OFF)
option(BUILD_EXAMPLES "Build examples" OFF)

# Enable C++11 (or higher if preferred)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Add debug flags
if(MSVC)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /Zi /MP")
else()
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -Wall -Wextra -Weffc++")
endif()

if(APPLE)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -framework CoreServices")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsanitize=address")
endif()

# Dial up the warnings
if(MSVC)
  # Force to always compile with W4
  if(CMAKE_CXX_FLAGS MATCHES "/W[0-4]")
    string(REGEX REPLACE "/W[0-4]" "/W4" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
  else()
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4")
  endif()
elseif(CMAKE_COMPILER_IS_GNUCC OR CMAKE_COMPILER_IS_GNUCXX)
  # Update if necessary
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wno-long-long -pedantic")
endif()

# Set up testing
include(CTest)
if(BUILD_TESTS AND BUILD_TESTING)
  add_subdirectory(tests)
endif()

# Build examples if enabled
if(BUILD_EXAMPLES)
  add_subdirectory(example)
endif()

# Install target for header-only library
install(FILES FileWatch.hpp
        DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/filewatch)
