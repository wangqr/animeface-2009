cmake_minimum_required(VERSION 2.6)
project(nvxs)

add_library(nvxs SHARED
    nv_face/nv_face_analyze.c
    nv_face/nv_face_haarlike.c
    nv_face/nv_face_detect.c
    nv_face/nv_face_mlp_static.c
    nv_core/nv_core_matrix.c
    nv_core/nv_core_util.c
    nv_ml/nv_gaussian.c
    nv_ml/nv_kmeans.c
    nv_ml/nv_mlp.c
    nv_num/nv_cov.c
    nv_num/nv_eigen.c
    nv_num/nv_vector.c
    nv_num/nv_distance.c
    nv_num/nv_matrix.c
    nv_ip/nv_euclidean_color.c
    nv_ip/nv_gaussian5x5.c
    nv_ip/nv_gray.c
    nv_ip/nv_gray_cpu.c
    nv_ip/nv_integral.c
    nv_ip/nv_laplacian.c
)

set(NVXS_PUBLIC_HEADER
    nv_face/nv_face.h
    nv_face/nv_face_feature.h
    nv_face/nv_face_mlp_static.h
    nv_face/nv_face_analyze.h
    nv_face/nv_face_detect_internal.h
    nv_face/nv_face_detect.h
    nv_core/nv_config.h
    nv_core/nv_core.h
    nv_core/nv_core_matrix.h
    nv_core/nv_core_util.h
    nv_core/nv_portable.h
    nv_ip/nv_ip.h
    nv_ip/nv_ip_euclidean_color.h
    nv_ip/nv_ip_gaussian.h
    nv_ip/nv_ip_gray.h
    nv_ip/nv_ip_integral.h
    nv_ip/nv_ip_laplacian.h
    nv_num/nv_num.h
    nv_num/nv_num_cov.h
    nv_num/nv_num_distance.h
    nv_num/nv_num_eigen.h
    nv_num/nv_num_lapack.h
    nv_num/nv_num_matrix.h
    nv_num/nv_num_vector.h
    nv_ml/nv_ml.h
    nv_ml/nv_ml_gaussian.h
    nv_ml/nv_ml_kmeans.h
    nv_ml/nv_ml_mlp.h
    
    CLAPACK/INCLUDE/blaswrap.h CLAPACK/INCLUDE/clapack.h CLAPACK/INCLUDE/f2c.h
)

set_target_properties(nvxs PROPERTIES PUBLIC_HEADER "${NVXS_PUBLIC_HEADER}")

include_directories(nv_face nv_core nv_ip nv_num nv_ml)

#find_package(clapack REQUIRED)
#include_directories(${CLAPACK_INCLUDE_DIRS})
#target_link_libraries(nvxs ${CLAPACK_LIBRARY})
include_directories(CLAPACK/INCLUDE)
target_link_libraries(nvxs "${CMAKE_CURRENT_SOURCE_DIR}/CLAPACK/liblapack_nvxs.a" "${CMAKE_CURRENT_SOURCE_DIR}/CLAPACK/libblas_nvxs.a" "${CMAKE_CURRENT_SOURCE_DIR}/CLAPACK/libtmglib_nvxs.a" "${CMAKE_CURRENT_SOURCE_DIR}/CLAPACK/F2CLIBS/libf2c.a")

find_package(OpenMP)
if(OPENMP_FOUND)
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
endif()

if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    set( CMAKE_INSTALL_PREFIX "${CMAKE_CURRENT_SOURCE_DIR}/../install" CACHE PATH "Install path prefix, prepended onto install directories." FORCE)
endif()
install(TARGETS nvxs LIBRARY DESTINATION lib PUBLIC_HEADER DESTINATION include)