cmake_minimum_required(VERSION 3.15)
project(rubberband LANGUAGES C CXX)

# 基础配置
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# 优化选项
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -ffast-math -ftree-vectorize")
set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -ffast-math -ftree-vectorize")

# 源文件定义
set(LIB_SOURCES
    src/rubberband-c.cpp
    src/RubberBandStretcher.cpp
    src/faster/AudioCurveCalculator.cpp
    src/faster/CompoundAudioCurve.cpp
    src/faster/HighFrequencyAudioCurve.cpp
    src/faster/SilentAudioCurve.cpp
    src/faster/PercussiveAudioCurve.cpp
    src/faster/R2Stretcher.cpp
    src/faster/StretcherChannelData.cpp
    src/faster/StretcherProcess.cpp
    src/common/Allocators.cpp
    src/common/BQResampler.cpp
    src/common/FFT.cpp
    src/common/Log.cpp
    src/common/Profiler.cpp
    src/common/Resampler.cpp
    src/common/StretchCalculator.cpp
    src/common/sysutils.cpp
    src/common/mathmisc.cpp
    src/common/Thread.cpp
    src/finer/R3Stretcher.cpp
)

# 头文件包含
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/src
    ${CMAKE_CURRENT_SOURCE_DIR}/rubberband
)

# 预处理器定义
add_compile_definitions(
    USE_BQRESAMPLER
    USE_BUILTIN_FFT
    NO_THREAD_CHECKS
    USE_PTHREADS
    NO_TIMING
    HAVE_POSIX_MEMALIGN
    NDEBUG
)

# 构建静态库
add_library(rubberband STATIC ${LIB_SOURCES})

# 依赖项配置
find_package(Threads REQUIRED)
target_link_libraries(rubberband PUBLIC Threads::Threads)

install(TARGETS rubberband
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin  # Windows平台需要
    INCLUDES DESTINATION include/rubberband
)

install(FILES
    rubberband/rubberband-c.h
    rubberband/RubberBandStretcher.h
    DESTINATION include/rubberband
)