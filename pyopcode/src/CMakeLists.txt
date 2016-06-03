cmake_minimum_required (VERSION 3.5)
project (pyopcode)

set(Boost_USE_STATIC_LIBS OFF)
set(Boost_USE_MULTITHREADED ON)
find_package(Boost REQUIRED COMPONENTS python)

file(GLOB OPCODE_SOURCE opcode/*.cpp)
file(GLOB ICE_SOURCE opcode/Ice/*.cpp)

include_directories(opcode)
include_directories(opcode/Ice)

include_directories(${PYTHON_INCLUDE_DIR})
include_directories(${NUMPY_INCLUDE_DIR})
include_directories(${Boost_INCLUDE_DIR})

LINK_DIRECTORIES(${PYTHON_LIBRARY})
LINK_DIRECTORIES(${Boost_LIBRARIES})


ADD_LIBRARY(pyopcode MODULE interface.cpp ${OPCODE_SOURCE} ${ICE_SOURCE})
SET_TARGET_PROPERTIES(pyopcode PROPERTIES SUFFIX ".pyd")
target_compile_definitions(pyopcode PUBLIC ICE_NO_DLL BAN_OPCODE_AUTOLINK)

TARGET_LINK_LIBRARIES(pyopcode
    ${Boost_LIBRARIES}
    ${PYTHON_LIBRARY}
    )