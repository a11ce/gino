# Sources
set(Srcs 
  DOALL.cpp
  DOALLTask.cpp
  DOALL_analysis.cpp
  DOALL_lastIteration.cpp
  DOALL_applicabilityGuard.cpp
  DOALL_parallelization.cpp
  DOALL_chunking.cpp
  DOALL_linker.cpp
  DOALL_inputOutput.cpp
)

# Compilation flags
set_source_files_properties(${Srcs} PROPERTIES COMPILE_FLAGS " -std=c++17 -fPIC")

# Name of the LLVM pass
set(PassName "DOALL")

set(LLVM_RUNTIME_OUTPUT_INTDIR ${CMAKE_BINARY_DIR}/${CMAKE_CFG_INTDIR}/)
set(LLVM_LIBRARY_OUTPUT_INTDIR ${CMAKE_BINARY_DIR}/${CMAKE_CFG_INTDIR}/)

list(APPEND CMAKE_MODULE_PATH "${LLVM_CMAKE_DIR}")
include(HandleLLVMOptions)
include(AddLLVM)

message(STATUS "LLVM_DIR IS ${LLVM_CMAKE_DIR}.")

include_directories(${LLVM_INCLUDE_DIRS} 
  ../../heuristics/include 
  ../../parallelization_technique/include 
  ../include 
  ./ 
  ${CMAKE_INSTALL_PREFIX}/include
  )

# Declare the LLVM pass to compile
add_llvm_library(${PassName} MODULE ${Srcs})
