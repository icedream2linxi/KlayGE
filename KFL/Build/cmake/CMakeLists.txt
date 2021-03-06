PROJECT(KFL)

CMAKE_MINIMUM_REQUIRED(VERSION 2.8.6)

IF(COMMAND cmake_policy)
	CMAKE_POLICY(SET CMP0003 NEW)
ENDIF(COMMAND cmake_policy)

SET(KFL_MAJOR_VERSION 4)
SET(KFL_MINOR_VERSION 7)
SET(KFL_PATCH_VERSION 0)
SET(KFL_VERSION ${KFL_MAJOR_VERSION}.${KFL_MINOR_VERSION}.${KFL_PATCH_VERSION})

SET(KFL_PROJECT_DIR "${CMAKE_CURRENT_SOURCE_DIR}/../..")
SET(KLAYGE_ROOT_DIR "${KFL_PROJECT_DIR}/..")

INCLUDE(${KLAYGE_ROOT_DIR}/cmake/Common.cmake)
INCLUDE(${KLAYGE_ROOT_DIR}/cmake/Platform.cmake)
INCLUDE(${KLAYGE_ROOT_DIR}/cmake/Compiler.cmake)
INCLUDE(${KLAYGE_ROOT_DIR}/cmake/ConfigBoost.cmake)

SET_PROPERTY(GLOBAL PROPERTY USE_FOLDERS ON)

SET(KFL_OUTPUT_DIR "${KFL_PROJECT_DIR}/lib/${KLAYGE_PLATFORM_NAME}")

# The project settings

SET(LIB_NAME KFL)

SET(KERNEL_DETAIL_HEADER_FILES
	${KFL_PROJECT_DIR}/include/KFL/Detail/AutoLink.hpp
)
SET(KERNEL_CXX17_HEADER_FILES
	${KFL_PROJECT_DIR}/include/KFL/CXX17/any.hpp
	${KFL_PROJECT_DIR}/include/KFL/CXX17/filesystem.hpp
	${KFL_PROJECT_DIR}/include/KFL/CXX17/optional.hpp
)
SET(KERNEL_HEADER_FILES
	${KFL_PROJECT_DIR}/include/KFL/AlignedAllocator.hpp
	${KFL_PROJECT_DIR}/include/KFL/COMPtr.hpp
	${KFL_PROJECT_DIR}/include/KFL/Config.hpp
	${KFL_PROJECT_DIR}/include/KFL/CpuInfo.hpp
	${KFL_PROJECT_DIR}/include/KFL/CustomizedStreamBuf.hpp
	${KFL_PROJECT_DIR}/include/KFL/CXX11.hpp
	${KFL_PROJECT_DIR}/include/KFL/DllLoader.hpp
	${KFL_PROJECT_DIR}/include/KFL/Hash.hpp
	${KFL_PROJECT_DIR}/include/KFL/KFL.hpp
	${KFL_PROJECT_DIR}/include/KFL/Log.hpp
	${KFL_PROJECT_DIR}/include/KFL/PreDeclare.hpp
	${KFL_PROJECT_DIR}/include/KFL/ResIdentifier.hpp
	${KFL_PROJECT_DIR}/include/KFL/Thread.hpp
	${KFL_PROJECT_DIR}/include/KFL/ThrowErr.hpp
	${KFL_PROJECT_DIR}/include/KFL/Timer.hpp
	${KFL_PROJECT_DIR}/include/KFL/Trace.hpp
	${KFL_PROJECT_DIR}/include/KFL/Types.hpp
	${KFL_PROJECT_DIR}/include/KFL/Util.hpp
	${KFL_PROJECT_DIR}/include/KFL/XMLDom.hpp
)
SET(KERNEL_SOURCE_FILES
	${KFL_PROJECT_DIR}/src/Kernel/CpuInfo.cpp
	${KFL_PROJECT_DIR}/src/Kernel/CustomizedStreamBuf.cpp
	${KFL_PROJECT_DIR}/src/Kernel/DllLoader.cpp
	${KFL_PROJECT_DIR}/src/Kernel/KFL.cpp
	${KFL_PROJECT_DIR}/src/Kernel/Log.cpp
	${KFL_PROJECT_DIR}/src/Kernel/ThrowErr.cpp
	${KFL_PROJECT_DIR}/src/Kernel/Thread.cpp
	${KFL_PROJECT_DIR}/src/Kernel/Timer.cpp
	${KFL_PROJECT_DIR}/src/Kernel/Util.cpp
	${KFL_PROJECT_DIR}/src/Kernel/XMLDom.cpp
)

SET(MATH_HEADER_FILES
	${KFL_PROJECT_DIR}/include/KFL/Detail/MathHelper.hpp
	${KFL_PROJECT_DIR}/include/KFL/AABBox.hpp
	${KFL_PROJECT_DIR}/include/KFL/Bound.hpp
	${KFL_PROJECT_DIR}/include/KFL/Color.hpp
	${KFL_PROJECT_DIR}/include/KFL/Frustum.hpp
	${KFL_PROJECT_DIR}/include/KFL/Half.hpp
	${KFL_PROJECT_DIR}/include/KFL/Math.hpp
	${KFL_PROJECT_DIR}/include/KFL/Matrix.hpp
	${KFL_PROJECT_DIR}/include/KFL/Noise.hpp
	${KFL_PROJECT_DIR}/include/KFL/OBBox.hpp
	${KFL_PROJECT_DIR}/include/KFL/Plane.hpp
	${KFL_PROJECT_DIR}/include/KFL/Quaternion.hpp
	${KFL_PROJECT_DIR}/include/KFL/Rect.hpp
	${KFL_PROJECT_DIR}/include/KFL/SIMDMath.hpp
	${KFL_PROJECT_DIR}/include/KFL/SIMDMatrix.hpp
	${KFL_PROJECT_DIR}/include/KFL/SIMDVector.hpp
	${KFL_PROJECT_DIR}/include/KFL/Size.hpp
	${KFL_PROJECT_DIR}/include/KFL/Sphere.hpp
	${KFL_PROJECT_DIR}/include/KFL/Vector.hpp
)
SET(MATH_SOURCE_FILES
	${KFL_PROJECT_DIR}/src/Math/AABBox.cpp
	${KFL_PROJECT_DIR}/src/Math/Color.cpp
	${KFL_PROJECT_DIR}/src/Math/Frustum.cpp
	${KFL_PROJECT_DIR}/src/Math/Half.cpp
	${KFL_PROJECT_DIR}/src/Math/Math.cpp
	${KFL_PROJECT_DIR}/src/Math/Matrix.cpp
	${KFL_PROJECT_DIR}/src/Math/Noise.cpp
	${KFL_PROJECT_DIR}/src/Math/OBBox.cpp
	${KFL_PROJECT_DIR}/src/Math/Plane.cpp
	${KFL_PROJECT_DIR}/src/Math/Quaternion.cpp
	${KFL_PROJECT_DIR}/src/Math/Rect.cpp
	${KFL_PROJECT_DIR}/src/Math/SIMDMath.cpp
	${KFL_PROJECT_DIR}/src/Math/SIMDMatrix.cpp
	${KFL_PROJECT_DIR}/src/Math/SIMDVector.cpp
	${KFL_PROJECT_DIR}/src/Math/Size.cpp
	${KFL_PROJECT_DIR}/src/Math/Sphere.cpp
)

SOURCE_GROUP("Kernel\\Source Files" FILES ${KERNEL_SOURCE_FILES})
SOURCE_GROUP("Kernel\\Header Files" FILES ${KERNEL_HEADER_FILES})
SOURCE_GROUP("Kernel\\Header Files\\Detail" FILES ${KERNEL_DETAIL_HEADER_FILES})
SOURCE_GROUP("Kernel\\Header Files\\CXX17" FILES ${KERNEL_CXX17_HEADER_FILES})
SOURCE_GROUP("Math\\Source Files" FILES ${MATH_SOURCE_FILES})
SOURCE_GROUP("Math\\Header Files" FILES ${MATH_HEADER_FILES})

ADD_DEFINITIONS(-DKFL_SOURCE)

INCLUDE_DIRECTORIES(${Boost_INCLUDE_DIR})
INCLUDE_DIRECTORIES(${KFL_PROJECT_DIR}/../External/rapidxml)
INCLUDE_DIRECTORIES(${KFL_PROJECT_DIR}/include)
LINK_DIRECTORIES(${Boost_LIBRARY_DIR})
ADD_LIBRARY(${LIB_NAME} STATIC
	${KERNEL_SOURCE_FILES} ${KERNEL_HEADER_FILES} ${KERNEL_DETAIL_HEADER_FILES} ${KERNEL_CXX17_HEADER_FILES}
	${MATH_SOURCE_FILES} ${MATH_HEADER_FILES}
)

SET(LIB_OUTPUT_NAME ${LIB_NAME}${KLAYGE_OUTPUT_SUFFIX})

SET_TARGET_PROPERTIES(${LIB_NAME} PROPERTIES
	ARCHIVE_OUTPUT_DIRECTORY ${KFL_OUTPUT_DIR}
	ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${KFL_OUTPUT_DIR}
	ARCHIVE_OUTPUT_DIRECTORY_RELEASE ${KFL_OUTPUT_DIR}
	ARCHIVE_OUTPUT_DIRECTORY_RELWITHDEBINFO ${KFL_OUTPUT_DIR}
	ARCHIVE_OUTPUT_DIRECTORY_MINSIZEREL ${KFL_OUTPUT_DIR}
	PROJECT_LABEL ${LIB_NAME}
	DEBUG_POSTFIX ${CMAKE_DEBUG_POSTFIX}
	OUTPUT_NAME ${LIB_OUTPUT_NAME}
)

ADD_PRECOMPILED_HEADER(${LIB_NAME} "KFL/KFL.hpp" "${KFL_PROJECT_DIR}/include" "${KFL_PROJECT_DIR}/src/Kernel/KFL.cpp")

TARGET_LINK_LIBRARIES(${LIB_NAME})
