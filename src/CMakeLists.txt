
IF(NOT WIN32)
find_package(JPEG)
ENDIF(NOT WIN32)

IF(WIN32)
SET(JPEG_LIBRARY jpeg)
ENDIF(WIN32)

IF(WIN32)
SET(MATH_LIBS clapack cblas cminpack f2c)
ELSE(WIN32)
#Clapack stuff is not the same under unix system
SET(MATH_LIBS lapack cblas cminpack -lgfortran)
ENDIF(WIN32)

#Detect OpenMP
FIND_PACKAGE(OpenMP) 
if (OPENMP_FOUND) 
  SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
  option(USE_OPENMP "Use OpenMP for parallelization" ON)
else(OPENMP_FOUND)
  option(USE_OPENMP "Use OpenMP for parallelization" OFF)
endif (OPENMP_FOUND)

ADD_EXECUTABLE(KeyMatchFull KeyMatchFull.cpp keys2a.cpp)
TARGET_LINK_LIBRARIES(KeyMatchFull ann_1.1_char zlib)

ADD_EXECUTABLE(RadialUndistort RadialUndistort.cpp LoadJPEG.cpp)
TARGET_LINK_LIBRARIES(RadialUndistort imagelib matrix ${JPEG_LIBRARY} ${MATH_LIBS})

ADD_EXECUTABLE(Bundle2Vis Bundle2Vis.cpp)

ADD_EXECUTABLE(Bundle2PMVS Bundle2PMVS.cpp LoadJPEG.cpp)
TARGET_LINK_LIBRARIES(Bundle2PMVS imagelib matrix ${JPEG_LIBRARY} zlib ${MATH_LIBS})

SET(BUNDLER_SOURCES BaseApp.cpp BaseApp.h BundlerApp.cpp BundlerApp.h keys.cpp keys.h Register.cpp Register.h Epipolar.cpp Epipolar.h	
	Bundle.cpp Bundle.h BundleFast.cpp MatchTracks.cpp Camera.cpp Camera.h Geometry.cpp Geometry.h
	ImageData.cpp ImageData.h SifterUtil.cpp SifterUtil.h BaseGeometry.cpp BundlerGeometry.cpp
	BoundingBox.cpp BoundingBox.h BundleAdd.cpp BundleAdd.h ComputeTracks.cpp BruteForceSearch.cpp BruteForceSearch.h
	BundleIO.cpp ProcessBundle.cpp BundleTwo.cpp Decompose.cpp Decompose.h
	RelativePose.cpp Distortion.cpp Distortion.h TwoFrameModel.cpp TwoFrameModel.h LoadJPEG.cpp LoadJPEG.h)
SET_SOURCE_FILES_PROPERTIES(${BUNDLER_SOURCES}
  PROPERTIES
  COMPILE_FLAGS "-D__NO_UI__ -D__BUNDLER__ -D__BUNDLER_DISTR__ -D_CRT_SECURE_NO_WARNINGS")
ADD_EXECUTABLE(Bundler ${BUNDLER_SOURCES})
TARGET_LINK_LIBRARIES(Bundler imagelib sfm-driver sba-1.5 matrix zlib
 5point ${JPEG_LIBRARY} ann_1.1_char getopt ${MATH_LIBS} pbaGPU)

set(CMAKE_VERBOSE_MAKEFILE ON)
