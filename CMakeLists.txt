cmake_minimum_required (VERSION 2.6)
project(QMath)

# The version number.
set (QMATH_VERSION_MAJOR 1)
set (QMATH_VERSION_MINOR 0)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file (
  "${PROJECT_SOURCE_DIR}/QMathConfig.h.in"
  "${PROJECT_BINARY_DIR}/QMathConfig.h"
  )

# add the binary tree to the search path for include files
# so that we will find QMathConfig.h
include_directories("${PROJECT_BINARY_DIR}")
set(LIBQMATH_HDR 
	VectorBase.hpp
	Vector.hpp)

set(LIBQMATH_SRC 
	MathException.cpp 
	Transform.cpp)

# should we use our own math functions?s
option (ENABLE_GSL "Enable Gnu Scientific Library (GSL) support" ON) 

# enable gsl library?
#
if (ENABLE_GSL)
  include_directories ("${PROJECT_SOURCE_DIR}/GSLCompat")
  add_subdirectory (GSLCompat)
  set (EXTRA_LIBS ${EXTRA_LIBS} QMathGSL)
endif (ENABLE_GSL)
 
# add the library target

add_library(qmath_dynamic SHARED ${LIBQMATH_SRC})
add_library(qmath_static STATIC ${LIBQMATH_SRC})

set_target_properties(qmath_dynamic PROPERTIES OUTPUT_NAME "QMath")
set_target_properties(qmath_dynamic PROPERTIES VERSION ${QMATH_VERSION_MAJOR}.${QMATH_VERSION_MINOR} SOVERSION ${QMATH_VERSION_MAJOR})

set_target_properties(qmath_static PROPERTIES OUTPUT_NAME "QMath")

# add the install targets
install(TARGETS qmath_dynamic qmath_static LIBRARY DESTINATION lib ARCHIVE DESTINATION lib)

install (FILES "${PROJECT_BINARY_DIR}/QMathConfig.h" DESTINATION include)
install (FILES ${LIBQMATH_HDR} DESTINATION include)


