
INCLUDE_DIRECTORIES( "../imagelib/" "../matrix")

# list sources files
FILE(GLOB LIBRARY_FILES_C "./*.c")
FILE(GLOB LIBRARY_FILES_H "./*.h")

add_library(5point 
5point.c poly1.c poly3.c 
5point.h poly1.h poly3.h)
# set_source_files_properties(5point PROPERTIES LANGUAGE CXX)