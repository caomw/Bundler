
INCLUDE_DIRECTORIES( "../matrix")



project( imagelib CXX )

set(imagelib_srcs
imagelib.c affine.c bmp.c canny.c color.c fileio.c filter.c fit.c fmatrix.c homography.c horn.c image.c lerp.c morphology.c pgm.c poly.c qsort.c resample.c tps.c transform.c triangulate.c util.c
imagelib.h affine.h bmp.h canny.h color.h fileio.h filter.h fit.h fmatrix.h homography.h horn.h image.h lerp.h morphology.h pgm.h poly.h qsort.h resample.h tps.h transform.h triangulate.h util.h
)

add_library( imagelib STATIC ${imagelib_srcs} )
# set_target_properties ( imagelib PROPERTIES LINKER_LANGUAGE CXX )
# set_target_properties ( imagelib PROPERTIES LANGUAGE CXX )