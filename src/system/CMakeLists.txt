# add_library(system INTERFACE)
# target_include_directories(system
#     INTERFACE
#     ${CMAKE_CURRENT_SOURCE_DIR}
# )
# set(METHOD_LIBRARY classical_ising.cpp quantum_ising.cpp)

# if(USE_CUDA)
# 	set(METHOD_LIBRARY ${METHOD_LIBRARY} chimera_gpu/chimera_gpu_quantum.cpp chimera_gpu/cuda/kernel.cu)
# endif()

# add_library(system SHARED ${METHOD_LIBRARY})
# add_library(system_static STATIC ${METHOD_LIBRARY})

# if(USE_CUDA)
# 	target_include_directories(system PRIVATE ${CMAKE_CUDA_TOOLKIT_INCLUDE_DIRECTORIES})
# 	target_include_directories(system_static PRIVATE ${CMAKE_CUDA_TOOLKIT_INCLUDE_DIRECTORIES})
#
# 	set_target_properties(system PROPERTIES
# 		POSITION_INDEPENDENT_CODE ON
# 		CUDA_VISIBILITY_PRESET "hidden"
# 		# CUDA_SEPARABLE_COMPILATION ON
# 		)
# 	set_target_properties(system_static PROPERTIES
# 		POSITION_INDEPENDENT_CODE ON
# 		CUDA_VISIBILITY_PRESET "hidden"
# 		# CUDA_SEPARABLE_COMPILATION ON
# 		)
# endif()

# target_link_libraries(system PRIVATE algorithm graph)
# target_link_libraries(system_static PRIVATE algorithm_static graph_static)

# if(USE_CUDA)
# 	target_link_libraries(system PRIVATE ${CUDA_curand_LIBRARY})
# 	target_link_libraries(system_static PRIVATE ${CUDA_curand_LIBRARY})
# endif()


if(USE_CUDA)

    find_package(CUDA REQUIRED)

    # include cuda toolkit directory and link with cuda library.
    target_include_directories(cxxjij_header_only INTERFACE ${CMAKE_CUDA_TOOLKIT_INCLUDE_DIRECTORIES})
    add_library(cxxjij_chimera_gpu_kernel_static STATIC "${PROJECT_SOURCE_DIR}\/src\/system\/gpu\/chimera_cuda\/kernel.cu")
    add_library(cxxjij_chimera_gpu_kernel SHARED "${PROJECT_SOURCE_DIR}\/src\/system\/gpu\/chimera_cuda\/kernel.cu")

 	set_target_properties(cxxjij_chimera_gpu_kernel_static PROPERTIES
        POSITION_INDEPENDENT_CODE ON
        #CUDA_VISIBILITY_PRESET "hidden"
        CUDA_SEPARABLE_COMPILATION ON
           )

 	set_target_properties(cxxjij_chimera_gpu_kernel PROPERTIES
        POSITION_INDEPENDENT_CODE ON
        #CUDA_VISIBILITY_PRESET "hidden"
        CUDA_SEPARABLE_COMPILATION ON
 		)

 	target_link_libraries(cxxjij_chimera_gpu_kernel_static ${CUDA_curand_LIBRARY} ${CUDA_CUBLAS_LIBRARIES} ${CUDA_LIBRARIES})
 	target_link_libraries(cxxjij_chimera_gpu_kernel_static cxxjij_header_only)
 	target_link_libraries(cxxjij_chimera_gpu_kernel ${CUDA_curand_LIBRARY} ${CUDA_CUBLAS_LIBRARIES} ${CUDA_LIBRARIES})
 	target_link_libraries(cxxjij_chimera_gpu_kernel cxxjij_header_only)
   set_property(TARGET cxxjij_chimera_gpu_kernel_static PROPERTY CUDA_STANDARD 11) 
   set_property(TARGET cxxjij_chimera_gpu_kernel PROPERTY CUDA_STANDARD 11) 

endif()