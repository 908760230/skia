cmake_minimum_required(VERSION 3.16 FATAL_ERROR)
cmake_policy(VERSION 3.16)
project(Skia LANGUAGES C CXX)

# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# 在 Windows 上使用动态运行时库
if(WIN32)
    set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>DLL")
endif()

# 定义必要的宏
add_definitions(-DSK_SHAPER_HARFBUZZ_AVAILABLE)
add_definitions(-DSK_SHAPER_UNICODE_AVAILABLE)
add_definitions(-DSK_UNICODE_ICU_IMPLEMENTATION)
add_definitions(-DSK_USING_THIRD_PARTY_ICU)
add_definitions(-DSKUNICODE_IMPLEMENTATION=1)
# ICU 静态链接宏定义
add_definitions(-DU_STATIC_IMPLEMENTATION)
add_definitions(-DU_COMMON_IMPLEMENTATION)
add_definitions(-DU_I18N_IMPLEMENTATION)
add_definitions(-DU_ENABLE_DYLOAD=0)
add_definitions(-DU_DISABLE_RENAMING=1)
add_definitions(-DU_USING_ICU_NAMESPACE=0)
add_definitions(-D_XOPEN_SOURCE=0)
add_definitions(-DU_NOEXCEPT=)

# 设置 CMake 变量
set(SKIA_ROOT_DIR ${CMAKE_CURRENT_SOURCE_DIR})
set(SKIA_SRC_DIR ${SKIA_ROOT_DIR}/src)
set(SKIA_INCLUDE_DIR ${SKIA_ROOT_DIR}/include)

# 添加 include 目录
include_directories(${SKIA_ROOT_DIR})
include_directories(${SKIA_INCLUDE_DIR})
include_directories(${SKIA_SRC_DIR}/gpu)
include_directories(${SKIA_SRC_DIR}/gpu/ganesh)
include_directories(${SKIA_ROOT_DIR}/modules/skcms)
include_directories(${SKIA_ROOT_DIR}/third_party/externals/d3d12allocator/src)
# 添加第三方库 include 目录
include_directories(${SKIA_ROOT_DIR}/third_party/externals/libpng)
include_directories(${SKIA_ROOT_DIR}/third_party/libpng)
include_directories(${SKIA_ROOT_DIR}/third_party/externals/libjpeg-turbo)
include_directories(${SKIA_ROOT_DIR}/third_party/externals/libjpeg-turbo/src)
include_directories(${SKIA_ROOT_DIR}/third_party/externals/zlib)
# 添加 ICU include 目录
include_directories(${SKIA_ROOT_DIR}/third_party/externals/icu/source/common)
include_directories(${SKIA_ROOT_DIR}/third_party/externals/icu/source/i18n)
include_directories(${SKIA_ROOT_DIR}/third_party/icu)

# 收集头文件
file(GLOB_RECURSE base_HEADERS
    ${SKIA_SRC_DIR}/base/*.h
)

file(GLOB_RECURSE core_HEADERS
    ${SKIA_SRC_DIR}/core/*.h
)

file(GLOB_RECURSE codec_HEADERS
    ${SKIA_SRC_DIR}/codec/*.h
)

file(GLOB_RECURSE utils_HEADERS
    ${SKIA_SRC_DIR}/utils/*.h
)

file(GLOB_RECURSE ports_HEADERS
    ${SKIA_SRC_DIR}/ports/*.h
)

file(GLOB_RECURSE effects_HEADERS
    ${SKIA_SRC_DIR}/effects/*.h
)

file(GLOB_RECURSE gpu_HEADERS
    ${SKIA_SRC_DIR}/gpu/*.h
    ${SKIA_INCLUDE_DIR}/gpu/*.h
)

file(GLOB_RECURSE skcms_HEADERS
    ${SKIA_ROOT_DIR}/modules/skcms/*.h
)

# 基础模块
set(base_SOURCES
    ${SKIA_SRC_DIR}/base/SkArenaAlloc.cpp
    ${SKIA_SRC_DIR}/base/SkBase64.cpp
    ${SKIA_SRC_DIR}/base/SkBezierCurves.cpp
    ${SKIA_SRC_DIR}/base/SkBuffer.cpp
    ${SKIA_SRC_DIR}/base/SkContainers.cpp
    ${SKIA_SRC_DIR}/base/SkCubics.cpp
    ${SKIA_SRC_DIR}/base/SkDebug.cpp
    ${SKIA_SRC_DIR}/base/SkDeque.cpp
    ${SKIA_SRC_DIR}/base/SkFloatingPoint.cpp
    ${SKIA_SRC_DIR}/base/SkHalf.cpp
    ${SKIA_SRC_DIR}/base/SkMathPriv.cpp
    ${SKIA_SRC_DIR}/base/SkMalloc.cpp
    ${SKIA_SRC_DIR}/base/SkQuads.cpp
    ${SKIA_SRC_DIR}/base/SkSafeMath.cpp
    ${SKIA_SRC_DIR}/base/SkSemaphore.cpp
    ${SKIA_SRC_DIR}/base/SkSharedMutex.cpp
    ${SKIA_SRC_DIR}/base/SkSpinlock.cpp
    ${SKIA_SRC_DIR}/base/SkTDArray.cpp
    ${SKIA_SRC_DIR}/base/SkTSearch.cpp
    ${SKIA_SRC_DIR}/base/SkThreadID.cpp
    ${SKIA_SRC_DIR}/base/SkTime.cpp
    ${SKIA_SRC_DIR}/base/SkUTF.cpp
    ${SKIA_SRC_DIR}/base/SkUtils.cpp
    ${SKIA_SRC_DIR}/base/SkBlockAllocator.cpp
    ${base_HEADERS}
)

# 核心模块
set(core_SOURCES
    ${SKIA_SRC_DIR}/core/SkAAClip.cpp
    ${SKIA_SRC_DIR}/core/SkATrace.cpp
    ${SKIA_SRC_DIR}/core/SkAlphaRuns.cpp
    ${SKIA_SRC_DIR}/core/SkAnalyticEdge.cpp
    ${SKIA_SRC_DIR}/core/SkAnnotation.cpp
    ${SKIA_SRC_DIR}/core/SkBBHFactory.cpp
    ${SKIA_SRC_DIR}/core/SkBigPicture.cpp
    ${SKIA_SRC_DIR}/core/SkBitmap.cpp
    ${SKIA_SRC_DIR}/core/SkBitmapCache.cpp
    ${SKIA_SRC_DIR}/core/SkBitmapDevice.cpp
    ${SKIA_SRC_DIR}/core/SkBlendMode.cpp
    ${SKIA_SRC_DIR}/core/SkBlitter.cpp
    ${SKIA_SRC_DIR}/core/SkBlitter_ARGB32.cpp
    ${SKIA_SRC_DIR}/core/SkBlitter_A8.cpp
    ${SKIA_SRC_DIR}/core/SkBlurEngine.cpp
    ${SKIA_SRC_DIR}/core/SkBlurMask.cpp
    ${SKIA_SRC_DIR}/core/SkCachedData.cpp
    ${SKIA_SRC_DIR}/core/SkCanvas.cpp
    ${SKIA_SRC_DIR}/core/SkCanvasPriv.cpp
    ${SKIA_SRC_DIR}/core/SkCanvas_Raster.cpp
    ${SKIA_SRC_DIR}/core/SkCapabilities.cpp
    ${SKIA_SRC_DIR}/core/SkChecksum.cpp
    ${SKIA_SRC_DIR}/core/SkClipStack.cpp
    ${SKIA_SRC_DIR}/core/SkColor.cpp
    ${SKIA_SRC_DIR}/core/SkColorFilter.cpp
    ${SKIA_SRC_DIR}/core/SkColorSpace.cpp
    ${SKIA_SRC_DIR}/core/SkColorTable.cpp
    ${SKIA_SRC_DIR}/core/SkConvertPixels.cpp
    ${SKIA_SRC_DIR}/core/SkCpu.cpp
    ${SKIA_SRC_DIR}/core/SkCubicClipper.cpp
    ${SKIA_SRC_DIR}/core/SkCubicMap.cpp
    ${SKIA_SRC_DIR}/core/SkData.cpp
    ${SKIA_SRC_DIR}/core/SkDataTable.cpp
    ${SKIA_SRC_DIR}/core/SkDevice.cpp
    ${SKIA_SRC_DIR}/core/SkDocument.cpp
    ${SKIA_SRC_DIR}/core/SkDraw.cpp
    ${SKIA_SRC_DIR}/core/SkDrawBase.cpp
    ${SKIA_SRC_DIR}/core/SkDraw_atlas.cpp
    ${SKIA_SRC_DIR}/core/SkDraw_text.cpp
    ${SKIA_SRC_DIR}/core/SkDraw_vertices.cpp
    ${SKIA_SRC_DIR}/core/SkDrawable.cpp
    ${SKIA_SRC_DIR}/core/SkEdge.cpp
    ${SKIA_SRC_DIR}/core/SkEdgeBuilder.cpp
    ${SKIA_SRC_DIR}/core/SkEdgeClipper.cpp
    ${SKIA_SRC_DIR}/core/SkExecutor.cpp
    ${SKIA_SRC_DIR}/core/SkFlattenable.cpp
    ${SKIA_SRC_DIR}/core/SkFont.cpp
    ${SKIA_SRC_DIR}/core/SkFontStream.cpp
    ${SKIA_SRC_DIR}/core/SkFont_serial.cpp
    ${SKIA_SRC_DIR}/core/SkGaussFilter.cpp
    ${SKIA_SRC_DIR}/core/SkGeometry.cpp
    ${SKIA_SRC_DIR}/core/SkGlyph.cpp
    ${SKIA_SRC_DIR}/core/SkGraphics.cpp
    ${SKIA_SRC_DIR}/core/SkImageFilter.cpp
    ${SKIA_SRC_DIR}/core/SkImageInfo.cpp
    ${SKIA_SRC_DIR}/core/SkLatticeIter.cpp
    ${SKIA_SRC_DIR}/core/SkLineClipper.cpp
    ${SKIA_SRC_DIR}/core/SkM44.cpp
    ${SKIA_SRC_DIR}/core/SkMD5.cpp
    ${SKIA_SRC_DIR}/core/SkMask.cpp
    ${SKIA_SRC_DIR}/core/SkMaskCache.cpp
    ${SKIA_SRC_DIR}/core/SkMaskFilter.cpp
    ${SKIA_SRC_DIR}/core/SkMaskGamma.cpp
    ${SKIA_SRC_DIR}/core/SkMasks.cpp
    ${SKIA_SRC_DIR}/core/SkMatrix.cpp
    ${SKIA_SRC_DIR}/core/SkMatrixInvert.cpp
    ${SKIA_SRC_DIR}/core/SkMesh.cpp
    ${SKIA_SRC_DIR}/core/SkMipmap.cpp
    ${SKIA_SRC_DIR}/core/SkMipmapBuilder.cpp
    ${SKIA_SRC_DIR}/core/SkOpts.cpp
    ${SKIA_SRC_DIR}/core/SkPaint.cpp
    ${SKIA_SRC_DIR}/core/SkPaintPriv.cpp
    ${SKIA_SRC_DIR}/core/SkPath.cpp
    ${SKIA_SRC_DIR}/core/SkPathBuilder.cpp
    ${SKIA_SRC_DIR}/core/SkPathEffect.cpp
    ${SKIA_SRC_DIR}/core/SkPathMeasure.cpp
    ${SKIA_SRC_DIR}/core/SkPathPriv.cpp
    ${SKIA_SRC_DIR}/core/SkPathRaw.cpp
    ${SKIA_SRC_DIR}/core/SkPathRawShapes.cpp
    ${SKIA_SRC_DIR}/core/SkPathRef.cpp
    ${SKIA_SRC_DIR}/core/SkPathUtils.cpp
    ${SKIA_SRC_DIR}/core/SkPath_serial.cpp
    ${SKIA_SRC_DIR}/core/SkContourMeasure.cpp
    ${SKIA_SRC_DIR}/core/SkPicture.cpp
    ${SKIA_SRC_DIR}/core/SkPictureData.cpp
    ${SKIA_SRC_DIR}/core/SkPictureFlat.cpp
    ${SKIA_SRC_DIR}/core/SkPictureRecord.cpp
    ${SKIA_SRC_DIR}/core/SkPixelRef.cpp
    ${SKIA_SRC_DIR}/core/SkPixmap.cpp
    ${SKIA_SRC_DIR}/core/SkPixmapDraw.cpp
    ${SKIA_SRC_DIR}/core/SkPoint.cpp
    ${SKIA_SRC_DIR}/core/SkPoint3.cpp
    ${SKIA_SRC_DIR}/core/SkPtrRecorder.cpp
    ${SKIA_SRC_DIR}/core/SkQuadClipper.cpp
    ${SKIA_SRC_DIR}/core/SkRRect.cpp
    ${SKIA_SRC_DIR}/core/SkRSXform.cpp
    ${SKIA_SRC_DIR}/core/SkRTree.cpp
    ${SKIA_SRC_DIR}/core/SkRasterClip.cpp
    ${SKIA_SRC_DIR}/core/SkReadBuffer.cpp
    ${SKIA_SRC_DIR}/core/SkReadPixelsRec.cpp
    ${SKIA_SRC_DIR}/core/SkRecord.cpp
    ${SKIA_SRC_DIR}/core/SkRecordCanvas.cpp
    ${SKIA_SRC_DIR}/core/SkRecordDraw.cpp
    ${SKIA_SRC_DIR}/core/SkRecordOpts.cpp
    ${SKIA_SRC_DIR}/core/SkRecords.cpp
    ${SKIA_SRC_DIR}/core/SkRect.cpp
    ${SKIA_SRC_DIR}/core/SkRegion.cpp
    ${SKIA_SRC_DIR}/core/SkRegion_path.cpp
    ${SKIA_SRC_DIR}/core/SkResourceCache.cpp
    ${SKIA_SRC_DIR}/core/SkRuntimeEffect.cpp
    ${SKIA_SRC_DIR}/core/SkSLTypeShared.cpp
    ${SKIA_SRC_DIR}/core/SkScalar.cpp
    ${SKIA_SRC_DIR}/core/SkScalerContext.cpp
    ${SKIA_SRC_DIR}/core/SkScan.cpp
    ${SKIA_SRC_DIR}/core/SkScan_AAAPath.cpp
    ${SKIA_SRC_DIR}/core/SkScan_AntiPath.cpp
    ${SKIA_SRC_DIR}/core/SkScan_Antihair.cpp
    ${SKIA_SRC_DIR}/core/SkScan_Hairline.cpp
    ${SKIA_SRC_DIR}/core/SkScan_Path.cpp
    ${SKIA_SRC_DIR}/core/SkSpecialImage.cpp
    ${SKIA_SRC_DIR}/core/SkStream.cpp
    ${SKIA_SRC_DIR}/core/SkStrike.cpp
    ${SKIA_SRC_DIR}/core/SkStrikeCache.cpp
    ${SKIA_SRC_DIR}/core/SkStrikeSpec.cpp
    ${SKIA_SRC_DIR}/core/SkString.cpp
    ${SKIA_SRC_DIR}/core/SkStringUtils.cpp
    ${SKIA_SRC_DIR}/core/SkTextBlob.cpp
    ${SKIA_SRC_DIR}/core/SkStroke.cpp
    ${SKIA_SRC_DIR}/core/SkStrokeRec.cpp
    ${SKIA_SRC_DIR}/core/SkStrokerPriv.cpp
    ${SKIA_SRC_DIR}/core/SkSwizzle.cpp
    ${SKIA_SRC_DIR}/core/SkSwizzler_opts.cpp
    ${SKIA_SRC_DIR}/core/SkSwizzler_opts_hsw.cpp
    ${SKIA_SRC_DIR}/core/SkSwizzler_opts_lasx.cpp
    ${SKIA_SRC_DIR}/core/SkSwizzler_opts_ssse3.cpp
    ${SKIA_SRC_DIR}/core/SkWriteBuffer.cpp
    ${SKIA_SRC_DIR}/core/SkTypeface.cpp
    ${SKIA_SRC_DIR}/core/SkTypefaceCache.cpp
    ${SKIA_SRC_DIR}/core/SkVertices.cpp
    ${SKIA_SRC_DIR}/core/SkWriter32.cpp
    ${SKIA_SRC_DIR}/core/SkMipmapAccessor.cpp
    ${SKIA_SRC_DIR}/core/SkMipmapDrawDownSampler.cpp
    ${SKIA_SRC_DIR}/core/SkMipmapHQDownSampler.cpp
    ${SKIA_SRC_DIR}/core/SkBitmapProcState.cpp
    ${SKIA_SRC_DIR}/core/SkBitmapProcState_opts.cpp
    ${SKIA_SRC_DIR}/core/SkBitmapProcState_opts_ssse3.cpp
    ${SKIA_SRC_DIR}/core/SkBitmapProcState_opts_lasx.cpp
    ${SKIA_SRC_DIR}/core/SkBlitMask_opts.cpp
    ${SKIA_SRC_DIR}/core/SkBlitMask_opts_ssse3.cpp
    ${SKIA_SRC_DIR}/core/SkBlitRow_opts.cpp
    ${SKIA_SRC_DIR}/core/SkBlitRow_opts_hsw.cpp
    ${SKIA_SRC_DIR}/core/SkBlitRow_opts_lasx.cpp
    ${SKIA_SRC_DIR}/core/SkMemset_opts.cpp
    ${SKIA_SRC_DIR}/core/SkSynchronizedResourceCache.cpp
    ${SKIA_SRC_DIR}/core/SkMaskBlurFilter.cpp
    ${SKIA_SRC_DIR}/core/SkBitmapProcState_matrixProcs.cpp
    ${SKIA_SRC_DIR}/core/SkKnownRuntimeEffects.cpp
    ${SKIA_SRC_DIR}/core/SkRasterPipeline.cpp
    ${SKIA_SRC_DIR}/core/SkRasterPipelineBlitter.cpp
    ${SKIA_SRC_DIR}/core/SkRuntimeBlender.cpp
    ${SKIA_SRC_DIR}/core/SkVertState.cpp
    ${SKIA_SRC_DIR}/core/SkBlitRow_D32.cpp
    ${SKIA_SRC_DIR}/core/SkMaskFilterBase.cpp
    ${SKIA_SRC_DIR}/core/SkBlurMaskFilterImpl.cpp
    ${SKIA_SRC_DIR}/core/SkGlyphRunPainter.cpp
    ${SKIA_SRC_DIR}/core/SkSpriteBlitter_ARGB32.cpp
    ${SKIA_SRC_DIR}/core/SkBlitter_Sprite.cpp
    ${SKIA_SRC_DIR}/core/SkPictureRecorder.cpp
    ${SKIA_SRC_DIR}/core/SkAutoPixmapStorage.cpp
    ${SKIA_SRC_DIR}/core/SkDescriptor.cpp
    ${SKIA_SRC_DIR}/core/SkPicturePlayback.cpp
    ${SKIA_SRC_DIR}/core/SkMemset_opts_avx.cpp
    ${SKIA_SRC_DIR}/core/SkMemset_opts_erms.cpp
    ${SKIA_SRC_DIR}/core/SkIDChangeListener.cpp
    ${SKIA_SRC_DIR}/core/SkRecordedDrawable.cpp
    ${SKIA_SRC_DIR}/core/SkDrawShadowInfo.cpp
    ${SKIA_SRC_DIR}/core/SkImageFilterTypes.cpp
    ${SKIA_SRC_DIR}/core/SkColorSpaceXformSteps.cpp
    ${SKIA_SRC_DIR}/core/SkUnPreMultiply.cpp
    ${SKIA_SRC_DIR}/core/SkFontDescriptor.cpp
    ${SKIA_SRC_DIR}/core/SkImageFilterCache.cpp
    ${SKIA_SRC_DIR}/core/SkLocalMatrixImageFilter.cpp
    ${SKIA_SRC_DIR}/core/SkImageGenerator.cpp
    ${SKIA_SRC_DIR}/core/SkCompressedDataUtils.cpp
    ${SKIA_SRC_DIR}/core/SkFontMgr.cpp
    ${SKIA_SRC_DIR}/core/SkYUVAPixmaps.cpp
    ${SKIA_SRC_DIR}/core/SkYUVAInfo.cpp
    ${SKIA_SRC_DIR}/core/SkMallocPixelRef.cpp
    ${SKIA_SRC_DIR}/core/SkWritePixelsRec.cpp
    ${SKIA_SRC_DIR}/core/SkBlendModeBlender.cpp
    ${SKIA_SRC_DIR}/core/SkYUVPlanesCache.cpp
    ${SKIA_SRC_DIR}/core/SkGlobalInitialization_core.cpp
    ${SKIA_SRC_DIR}/core/SkDistanceFieldGen.cpp
    ${SKIA_SRC_DIR}/core/SkTypeface_remote.cpp
    ${SKIA_SRC_DIR}/core/SkFontMetricsPriv.cpp
    ${SKIA_SRC_DIR}/core/SkTaskGroup.cpp
    ${SKIA_SRC_DIR}/core/SkYUVMath.cpp
    ${core_HEADERS}
)

# 编解码模块
set(codec_SOURCES
    ${SKIA_SRC_DIR}/codec/SkAndroidCodec.cpp
    ${SKIA_SRC_DIR}/codec/SkBmpBaseCodec.cpp
    ${SKIA_SRC_DIR}/codec/SkBmpCodec.cpp
    ${SKIA_SRC_DIR}/codec/SkBmpMaskCodec.cpp
    ${SKIA_SRC_DIR}/codec/SkBmpRLECodec.cpp
    ${SKIA_SRC_DIR}/codec/SkCodec.cpp
    ${SKIA_SRC_DIR}/codec/SkColorPalette.cpp
    ${SKIA_SRC_DIR}/codec/SkEncodedInfo.cpp
    ${SKIA_SRC_DIR}/codec/SkExif.cpp
    ${SKIA_SRC_DIR}/codec/SkGainmapInfo.cpp
    ${SKIA_SRC_DIR}/codec/SkIcoCodec.cpp
    ${SKIA_SRC_DIR}/codec/SkPixmapUtils.cpp
    ${SKIA_SRC_DIR}/codec/SkSampledCodec.cpp
    ${SKIA_SRC_DIR}/codec/SkSampler.cpp
    ${SKIA_SRC_DIR}/codec/SkTiffUtility.cpp
    ${SKIA_SRC_DIR}/codec/SkWbmpCodec.cpp
    ${SKIA_SRC_DIR}/codec/SkXmp.cpp
    ${SKIA_SRC_DIR}/codec/SkSwizzler.cpp
    ${SKIA_SRC_DIR}/codec/SkMaskSwizzler.cpp
    ${SKIA_SRC_DIR}/codec/SkImageGenerator_FromEncoded.cpp
    ${SKIA_SRC_DIR}/codec/SkPngCodec.cpp
    ${SKIA_SRC_DIR}/codec/SkGainmapInfo.cpp
    ${SKIA_SRC_DIR}/codec/SkPngCodecBase.cpp
    ${SKIA_SRC_DIR}/codec/SkPngCompositeChunkReader.cpp
    ${SKIA_SRC_DIR}/codec/SkCodecImageGenerator.cpp
    ${SKIA_SRC_DIR}/codec/SkBmpStandardCodec.cpp
    ${codec_HEADERS}
)

# Encode 模块
set(encode_SOURCES
    ${SKIA_SRC_DIR}/encode/SkEncoder.cpp
    ${SKIA_SRC_DIR}/encode/SkICC.cpp
    ${SKIA_SRC_DIR}/encode/SkPngEncoderImpl.cpp
    ${SKIA_SRC_DIR}/encode/SkPngEncoderBase.cpp
    ${SKIA_SRC_DIR}/encode/SkJpegEncoderImpl.cpp
    ${SKIA_SRC_DIR}/encode/SkJpegGainmapEncoder.cpp
    ${SKIA_SRC_DIR}/encode/SkJPEGWriteUtility.cpp
)

# 工具模块
set(utils_SOURCES
    ${SKIA_SRC_DIR}/utils/SkCamera.cpp
    ${SKIA_SRC_DIR}/utils/SkCanvasStack.cpp
    ${SKIA_SRC_DIR}/utils/SkCanvasStateUtils.cpp
    ${SKIA_SRC_DIR}/utils/SkCharToGlyphCache.cpp
    ${SKIA_SRC_DIR}/utils/SkClipStackUtils.cpp
    ${SKIA_SRC_DIR}/utils/SkCustomTypeface.cpp
    ${SKIA_SRC_DIR}/utils/SkDashPath.cpp
    ${SKIA_SRC_DIR}/utils/SkEventTracer.cpp
    ${SKIA_SRC_DIR}/utils/SkFloatToDecimal.cpp
    ${SKIA_SRC_DIR}/utils/SkJSONWriter.cpp
    ${SKIA_SRC_DIR}/utils/SkMatrix22.cpp
    ${SKIA_SRC_DIR}/utils/SkMultiPictureDocument.cpp
    ${SKIA_SRC_DIR}/utils/SkNWayCanvas.cpp
    ${SKIA_SRC_DIR}/utils/SkNullCanvas.cpp
    ${SKIA_SRC_DIR}/utils/SkOSPath.cpp
    ${SKIA_SRC_DIR}/utils/SkOrderedFontMgr.cpp
    ${SKIA_SRC_DIR}/utils/SkPaintFilterCanvas.cpp
    ${SKIA_SRC_DIR}/utils/SkParse.cpp
    ${SKIA_SRC_DIR}/utils/SkParseColor.cpp
    ${SKIA_SRC_DIR}/utils/SkParsePath.cpp
    ${SKIA_SRC_DIR}/utils/SkPatchUtils.cpp
    ${SKIA_SRC_DIR}/utils/SkPolyUtils.cpp
    ${SKIA_SRC_DIR}/utils/SkShaderUtils.cpp
    ${SKIA_SRC_DIR}/utils/SkShadowUtils.cpp
    ${SKIA_SRC_DIR}/utils/SkTextUtils.cpp
    ${SKIA_SRC_DIR}/utils/SkShadowTessellator.cpp
    ${utils_HEADERS}
)

# 平台特定的工具模块
if(WIN32)
    list(APPEND utils_SOURCES
        ${SKIA_SRC_DIR}/utils/SkGetExecutablePath_win.cpp
        ${SKIA_SRC_DIR}/utils/win/SkAutoCoInitialize.cpp
        ${SKIA_SRC_DIR}/utils/win/SkDWrite.cpp
        ${SKIA_SRC_DIR}/utils/win/SkDWriteFontFileStream.cpp
        ${SKIA_SRC_DIR}/utils/win/SkDWriteGeometrySink.cpp
        ${SKIA_SRC_DIR}/utils/win/SkHRESULT.cpp
        ${SKIA_SRC_DIR}/utils/win/SkIStream.cpp
        ${SKIA_SRC_DIR}/utils/win/SkWGL_win.cpp
    )
elseif(APPLE)
    list(APPEND utils_SOURCES
        ${SKIA_SRC_DIR}/utils/SkGetExecutablePath_mac.cpp
        ${SKIA_SRC_DIR}/utils/mac/SkCTFont.cpp
        ${SKIA_SRC_DIR}/utils/mac/SkCTFontCreateExactCopy.cpp
        ${SKIA_SRC_DIR}/utils/mac/SkCreateCGImageRef.cpp
    )
elseif(UNIX)
    list(APPEND utils_SOURCES
        ${SKIA_SRC_DIR}/utils/SkGetExecutablePath_linux.cpp
    )
endif()

# 平台特定模块
set(ports_SOURCES
    ${SKIA_SRC_DIR}/ports/SkDebug_stdio.cpp
    ${SKIA_SRC_DIR}/ports/SkDiscardableMemory_none.cpp
    ${SKIA_SRC_DIR}/ports/SkGlobalInitialization_default.cpp
    ${SKIA_SRC_DIR}/ports/SkMemory_malloc.cpp
    ${SKIA_SRC_DIR}/ports/SkOSFile_stdio.cpp
    ${ports_HEADERS}
)

# 平台特定的端口模块
if(WIN32)
    list(APPEND ports_SOURCES
        ${SKIA_SRC_DIR}/ports/SkDebug_win.cpp
        ${SKIA_SRC_DIR}/ports/SkFontHost_win.cpp
        ${SKIA_SRC_DIR}/ports/SkFontMgr_win_dw.cpp
        ${SKIA_SRC_DIR}/ports/SkImageGeneratorWIC.cpp
        ${SKIA_SRC_DIR}/ports/SkOSFile_win.cpp
        ${SKIA_SRC_DIR}/ports/SkScalerContext_win_dw.cpp
        ${SKIA_SRC_DIR}/ports/SkTypeface_win_dw.cpp
    )
elseif(APPLE)
    list(APPEND ports_SOURCES
        ${SKIA_SRC_DIR}/ports/SkFontMgr_mac_ct.cpp
        ${SKIA_SRC_DIR}/ports/SkImageGeneratorCG.cpp
        ${SKIA_SRC_DIR}/ports/SkScalerContext_mac_ct.cpp
        ${SKIA_SRC_DIR}/ports/SkTypeface_mac_ct.cpp
    )
elseif(UNIX)
    list(APPEND ports_SOURCES
        ${SKIA_SRC_DIR}/ports/SkDebug_android.cpp
        ${SKIA_SRC_DIR}/ports/SkFontConfigInterface.cpp
        ${SKIA_SRC_DIR}/ports/SkFontConfigInterface_direct.cpp
        ${SKIA_SRC_DIR}/ports/SkFontConfigInterface_direct_factory.cpp
        ${SKIA_SRC_DIR}/ports/SkFontHost_FreeType.cpp
        ${SKIA_SRC_DIR}/ports/SkFontHost_FreeType_common.cpp
        ${SKIA_SRC_DIR}/ports/SkFontMgr_FontConfigInterface.cpp
        ${SKIA_SRC_DIR}/ports/SkFontMgr_fontconfig.cpp
        ${SKIA_SRC_DIR}/ports/SkOSFile_posix.cpp
    )
endif()

# 效果模块
set(effects_SOURCES
    ${SKIA_SRC_DIR}/effects/Sk1DPathEffect.cpp
    ${SKIA_SRC_DIR}/effects/Sk2DPathEffect.cpp
    ${SKIA_SRC_DIR}/effects/SkBlenders.cpp
    ${SKIA_SRC_DIR}/effects/SkColorMatrix.cpp
    ${SKIA_SRC_DIR}/effects/SkColorMatrixFilter.cpp
    ${SKIA_SRC_DIR}/effects/SkCornerPathEffect.cpp
    ${SKIA_SRC_DIR}/effects/SkDashPathEffect.cpp
    ${SKIA_SRC_DIR}/effects/SkDiscretePathEffect.cpp
    ${SKIA_SRC_DIR}/effects/SkEmbossMaskFilter.cpp
    ${SKIA_SRC_DIR}/effects/SkEmbossMask.cpp
    ${SKIA_SRC_DIR}/effects/SkHighContrastFilter.cpp
    ${SKIA_SRC_DIR}/effects/SkShaderMaskFilterImpl.cpp
    ${SKIA_SRC_DIR}/effects/SkTableMaskFilter.cpp
    ${SKIA_SRC_DIR}/effects/SkTrimPathEffect.cpp
    # 颜色过滤器
    ${SKIA_SRC_DIR}/effects/colorfilters/SkBlendModeColorFilter.cpp
    ${SKIA_SRC_DIR}/effects/colorfilters/SkColorFilterBase.cpp
    ${SKIA_SRC_DIR}/effects/colorfilters/SkColorSpaceXformColorFilter.cpp
    ${SKIA_SRC_DIR}/effects/colorfilters/SkComposeColorFilter.cpp
    ${SKIA_SRC_DIR}/effects/colorfilters/SkGaussianColorFilter.cpp
    ${SKIA_SRC_DIR}/effects/colorfilters/SkMatrixColorFilter.cpp
    ${SKIA_SRC_DIR}/effects/colorfilters/SkRuntimeColorFilter.cpp
    ${SKIA_SRC_DIR}/effects/colorfilters/SkTableColorFilter.cpp
    ${SKIA_SRC_DIR}/effects/colorfilters/SkWorkingFormatColorFilter.cpp
    # 图像过滤器
    ${SKIA_SRC_DIR}/effects/imagefilters/SkBlendImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkBlurImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkColorFilterImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkComposeImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkCropImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkDisplacementMapImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkDropShadowImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkImageImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkLightingImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkMagnifierImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkMatrixConvolutionImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkMatrixTransformImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkMergeImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkMorphologyImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkPictureImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkRuntimeImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkShaderImageFilter.cpp
    ${effects_HEADERS}
)

# 着色器模块
set(shaders_SOURCES
    ${SKIA_SRC_DIR}/shaders/SkBitmapProcShader.cpp
    ${SKIA_SRC_DIR}/shaders/SkBlendShader.cpp
    ${SKIA_SRC_DIR}/shaders/SkColorFilterShader.cpp
    ${SKIA_SRC_DIR}/shaders/SkColorShader.cpp
    ${SKIA_SRC_DIR}/shaders/SkCoordClampShader.cpp
    ${SKIA_SRC_DIR}/shaders/SkEmptyShader.cpp
    ${SKIA_SRC_DIR}/shaders/SkGainmapShader.cpp
    ${SKIA_SRC_DIR}/shaders/SkImageShader.cpp
    ${SKIA_SRC_DIR}/shaders/SkLocalMatrixShader.cpp
    ${SKIA_SRC_DIR}/shaders/SkPerlinNoiseShaderImpl.cpp
    ${SKIA_SRC_DIR}/shaders/SkPictureShader.cpp
    ${SKIA_SRC_DIR}/shaders/SkRuntimeShader.cpp
    ${SKIA_SRC_DIR}/shaders/SkShader.cpp
    ${SKIA_SRC_DIR}/shaders/SkShaderBase.cpp
    ${SKIA_SRC_DIR}/shaders/SkTransformShader.cpp
    ${SKIA_SRC_DIR}/shaders/SkTriColorShader.cpp
    ${SKIA_SRC_DIR}/shaders/SkWorkingColorSpaceShader.cpp
    # 渐变着色器
    ${SKIA_SRC_DIR}/shaders/gradients/SkConicalGradient.cpp
    ${SKIA_SRC_DIR}/shaders/gradients/SkGradientBaseShader.cpp
    ${SKIA_SRC_DIR}/shaders/gradients/SkLinearGradient.cpp
    ${SKIA_SRC_DIR}/shaders/gradients/SkRadialGradient.cpp
    ${SKIA_SRC_DIR}/shaders/gradients/SkSweepGradient.cpp
)

# Image 模块
set(image_SOURCES
    ${SKIA_SRC_DIR}/image/SkImage.cpp
    ${SKIA_SRC_DIR}/image/SkImage_Base.cpp
    ${SKIA_SRC_DIR}/image/SkSurface.cpp
    ${SKIA_SRC_DIR}/image/SkSurface_Base.cpp
    ${SKIA_SRC_DIR}/image/SkSurface_Raster.cpp
    ${SKIA_SRC_DIR}/image/SkSurface_Null.cpp
    ${SKIA_SRC_DIR}/image/SkRescaleAndReadPixels.cpp
    ${SKIA_SRC_DIR}/image/SkImage_Raster.cpp
    ${SKIA_SRC_DIR}/image/SkImage_RasterFactories.cpp
    ${SKIA_SRC_DIR}/image/SkTiledImageUtils.cpp
    ${SKIA_SRC_DIR}/image/SkPictureImageGenerator.cpp
    ${SKIA_SRC_DIR}/image/SkImage_Lazy.cpp
    ${SKIA_SRC_DIR}/image/SkImage_LazyFactories.cpp
    ${SKIA_SRC_DIR}/image/SkImage_Picture.cpp
)

# SkSL 模块
set(sksl_SOURCES
    # 核心文件
    ${SKIA_SRC_DIR}/sksl/SkSLCompiler.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLContext.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLParser.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLLexer.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLModule.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLModuleLoader.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLModuleDataDefault.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLConstantFolder.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLAnalysis.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLBuiltinTypes.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLInliner.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLMangler.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLOperator.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLPool.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLString.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLUtil.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLErrorReporter.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLPosition.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLIntrinsicList.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLSampleUsage.cpp
    # IR 文件
    ${SKIA_SRC_DIR}/sksl/ir/SkSLBinaryExpression.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLBlock.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLChildCall.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLConstructor.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLConstructorArray.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLConstructorArrayCast.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLConstructorCompound.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLConstructorCompoundCast.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLConstructorDiagonalMatrix.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLConstructorMatrixResize.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLConstructorScalarCast.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLConstructorSplat.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLConstructorStruct.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLDiscardStatement.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLDoStatement.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLExpression.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLExpressionStatement.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLExtension.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLFieldAccess.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLForStatement.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLFunctionCall.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLFunctionDeclaration.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLFunctionDefinition.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLIfStatement.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLIndexExpression.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLInterfaceBlock.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLLayout.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLLiteral.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLModifierFlags.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLModifiersDeclaration.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLPostfixExpression.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLPrefixExpression.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLProgram.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLSetting.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLStructDefinition.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLSwitchCase.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLSwitchStatement.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLSwizzle.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLSymbol.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLSymbolTable.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLTernaryExpression.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLType.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLTypeReference.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLVarDeclarations.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLVariable.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLVariableReference.cpp
    # 分析文件
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLCanExitWithoutReturningValue.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLCheckProgramStructure.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLCheckSymbolTableCorrectness.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLFinalizationChecks.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLGetLoopControlFlowInfo.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLGetLoopUnrollInfo.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLGetReturnComplexity.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLHasSideEffects.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLIsConstantExpression.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLIsDynamicallyUniformExpression.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLIsSameExpressionTree.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLIsTrivialExpression.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLProgramUsage.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLReturnsInputAlpha.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLSpecialization.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLSwitchCaseContainsExit.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLSymbolTableStackBuilder.cpp
    # 变换文件
    ${SKIA_SRC_DIR}/sksl/transform/SkSLAddConstToVarModifiers.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLEliminateDeadFunctions.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLEliminateDeadGlobalVariables.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLEliminateDeadLocalVariables.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLEliminateEmptyStatements.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLEliminateUnnecessaryBraces.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLEliminateUnreachableCode.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLFindAndDeclareBuiltinFunctions.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLFindAndDeclareBuiltinStructs.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLFindAndDeclareBuiltinVariables.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLHoistSwitchVarDeclarationsAtTopLevel.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLRenamePrivateSymbols.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLReplaceConstVarsWithLiterals.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLReplaceSplatCastsWithSwizzles.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLRewriteIndexedSwizzle.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLTransform.cpp
    # 代码生成文件
    ${SKIA_SRC_DIR}/sksl/codegen/SkSLPipelineStageCodeGenerator.cpp
    ${SKIA_SRC_DIR}/sksl/codegen/SkSLRasterPipelineBuilder.cpp
    ${SKIA_SRC_DIR}/sksl/codegen/SkSLRasterPipelineCodeGenerator.cpp
    ${SKIA_SRC_DIR}/sksl/codegen/SkSLGLSLCodeGenerator.cpp
    # 追踪文件
    ${SKIA_SRC_DIR}/sksl/tracing/SkSLDebugTracePlayer.cpp
    ${SKIA_SRC_DIR}/sksl/tracing/SkSLDebugTracePriv.cpp
    ${SKIA_SRC_DIR}/sksl/tracing/SkSLTraceHook.cpp
)

# Image 模块
set(image_SOURCES
    ${SKIA_SRC_DIR}/image/SkImage.cpp
    ${SKIA_SRC_DIR}/image/SkImage_Base.cpp
    ${SKIA_SRC_DIR}/image/SkSurface.cpp
    ${SKIA_SRC_DIR}/image/SkSurface_Base.cpp
    ${SKIA_SRC_DIR}/image/SkSurface_Raster.cpp
    ${SKIA_SRC_DIR}/image/SkSurface_Null.cpp
    ${SKIA_SRC_DIR}/image/SkRescaleAndReadPixels.cpp
    ${SKIA_SRC_DIR}/image/SkImage_Raster.cpp
    ${SKIA_SRC_DIR}/image/SkImage_RasterFactories.cpp
    ${SKIA_SRC_DIR}/image/SkTiledImageUtils.cpp
    ${SKIA_SRC_DIR}/image/SkPictureImageGenerator.cpp
    ${SKIA_SRC_DIR}/image/SkImage_Lazy.cpp
    ${SKIA_SRC_DIR}/image/SkImage_LazyFactories.cpp
    ${SKIA_SRC_DIR}/image/SkImage_Picture.cpp
)

# sfnt 模块
set(sfnt_SOURCES
    ${SKIA_SRC_DIR}/sfnt/SkOTUtils.cpp
    ${SKIA_SRC_DIR}/sfnt/SkOTTable_name.cpp
)

# Encode 模块
set(encode_SOURCES
    ${SKIA_SRC_DIR}/encode/SkEncoder.cpp
    ${SKIA_SRC_DIR}/encode/SkICC.cpp
    ${SKIA_SRC_DIR}/encode/SkPngEncoderImpl.cpp
    ${SKIA_SRC_DIR}/encode/SkPngEncoderBase.cpp
    ${SKIA_SRC_DIR}/encode/SkJpegEncoderImpl.cpp
    ${SKIA_SRC_DIR}/encode/SkJpegGainmapEncoder.cpp
    ${SKIA_SRC_DIR}/encode/SkJPEGWriteUtility.cpp
)

# SkSL 模块
set(sksl_SOURCES
    # 核心文件
    ${SKIA_SRC_DIR}/sksl/SkSLCompiler.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLContext.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLParser.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLLexer.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLModule.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLModuleLoader.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLModuleDataDefault.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLConstantFolder.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLAnalysis.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLBuiltinTypes.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLInliner.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLMangler.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLOperator.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLPool.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLString.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLUtil.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLErrorReporter.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLPosition.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLIntrinsicList.cpp
    ${SKIA_SRC_DIR}/sksl/SkSLSampleUsage.cpp
    # IR 文件
    ${SKIA_SRC_DIR}/sksl/ir/SkSLBinaryExpression.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLBlock.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLChildCall.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLConstructor.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLConstructorArray.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLConstructorArrayCast.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLConstructorCompound.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLConstructorCompoundCast.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLConstructorDiagonalMatrix.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLConstructorMatrixResize.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLConstructorScalarCast.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLConstructorSplat.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLConstructorStruct.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLDiscardStatement.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLDoStatement.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLExpression.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLExpressionStatement.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLExtension.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLFieldAccess.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLForStatement.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLFunctionCall.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLFunctionDeclaration.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLFunctionDefinition.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLIfStatement.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLIndexExpression.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLInterfaceBlock.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLLayout.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLLiteral.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLModifierFlags.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLModifiersDeclaration.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLPostfixExpression.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLPrefixExpression.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLProgram.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLSetting.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLStructDefinition.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLSwitchCase.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLSwitchStatement.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLSwizzle.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLSymbol.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLSymbolTable.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLTernaryExpression.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLType.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLTypeReference.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLVarDeclarations.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLVariable.cpp
    ${SKIA_SRC_DIR}/sksl/ir/SkSLVariableReference.cpp
    # 分析文件
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLCanExitWithoutReturningValue.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLCheckProgramStructure.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLCheckSymbolTableCorrectness.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLFinalizationChecks.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLGetLoopControlFlowInfo.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLGetLoopUnrollInfo.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLGetReturnComplexity.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLHasSideEffects.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLIsConstantExpression.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLIsDynamicallyUniformExpression.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLIsSameExpressionTree.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLIsTrivialExpression.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLProgramUsage.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLReturnsInputAlpha.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLSpecialization.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLSwitchCaseContainsExit.cpp
    ${SKIA_SRC_DIR}/sksl/analysis/SkSLSymbolTableStackBuilder.cpp
    # 变换文件
    ${SKIA_SRC_DIR}/sksl/transform/SkSLAddConstToVarModifiers.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLEliminateDeadFunctions.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLEliminateDeadGlobalVariables.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLEliminateDeadLocalVariables.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLEliminateEmptyStatements.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLEliminateUnnecessaryBraces.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLEliminateUnreachableCode.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLFindAndDeclareBuiltinFunctions.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLFindAndDeclareBuiltinStructs.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLFindAndDeclareBuiltinVariables.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLHoistSwitchVarDeclarationsAtTopLevel.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLRenamePrivateSymbols.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLReplaceConstVarsWithLiterals.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLReplaceSplatCastsWithSwizzles.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLRewriteIndexedSwizzle.cpp
    ${SKIA_SRC_DIR}/sksl/transform/SkSLTransform.cpp
    # 代码生成文件
    ${SKIA_SRC_DIR}/sksl/codegen/SkSLPipelineStageCodeGenerator.cpp
    ${SKIA_SRC_DIR}/sksl/codegen/SkSLRasterPipelineBuilder.cpp
    ${SKIA_SRC_DIR}/sksl/codegen/SkSLRasterPipelineCodeGenerator.cpp
    ${SKIA_SRC_DIR}/sksl/codegen/SkSLGLSLCodeGenerator.cpp
    # 追踪文件
    ${SKIA_SRC_DIR}/sksl/tracing/SkSLDebugTracePlayer.cpp
    ${SKIA_SRC_DIR}/sksl/tracing/SkSLDebugTracePriv.cpp
    ${SKIA_SRC_DIR}/sksl/tracing/SkSLTraceHook.cpp
)



# GPU 模块
set(gpu_SOURCES
    # GPU 基础
    ${SKIA_SRC_DIR}/gpu/AtlasTypes.cpp
    ${SKIA_SRC_DIR}/gpu/Blend.cpp
    ${SKIA_SRC_DIR}/gpu/BlendFormula.cpp
    ${SKIA_SRC_DIR}/gpu/BlurUtils.cpp
    ${SKIA_SRC_DIR}/gpu/DataUtils.cpp
    ${SKIA_SRC_DIR}/gpu/DitherUtils.cpp
    ${SKIA_SRC_DIR}/gpu/MutableTextureState.cpp
    ${SKIA_SRC_DIR}/gpu/RectanizerPow2.cpp
    ${SKIA_SRC_DIR}/gpu/RectanizerSkyline.cpp
    ${SKIA_SRC_DIR}/gpu/ResourceKey.cpp
    ${SKIA_SRC_DIR}/gpu/ShaderErrorHandler.cpp
    ${SKIA_SRC_DIR}/gpu/SkBackingFit.cpp
    ${SKIA_SRC_DIR}/gpu/SkSLToBackend.cpp
    ${SKIA_SRC_DIR}/gpu/Swizzle.cpp
    ${SKIA_SRC_DIR}/gpu/TiledTextureUtils.cpp
    # Ganesh
    ${SKIA_SRC_DIR}/gpu/ganesh/ClipStack.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/Device.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/Device_drawTexture.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrAHardwareBufferUtils.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrAttachment.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrAuditTrail.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrBackendSemaphore.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrBackendSurface.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrBackendUtils.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrBlurUtils.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrBufferAllocPool.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrBufferUpdateRenderTask.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrCanvas.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrCaps.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrColorInfo.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrColorSpaceXform.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrContextThreadSafeProxy.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrContext_Base.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrCopyRenderTask.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDDLContext.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDDLTask.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDataUtils.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDefaultGeoProcFactory.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDeferredDisplayList.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDirectContext.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDirectContextPriv.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDrawOpAtlas.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDrawingManager.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDriverBugWorkarounds.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDynamicAtlas.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrEagerVertexAllocator.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrFixedClip.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrFragmentProcessor.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrFragmentProcessors.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrGeometryProcessor.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrGpu.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrGpuBuffer.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrGpuResource.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrImageContext.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrImageInfo.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrManagedResource.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrMemoryPool.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrMeshBuffers.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrMeshDrawTarget.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrOnFlushResourceProvider.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrOpFlushState.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrOpsRenderPass.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrPaint.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrPersistentCacheUtils.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrPipeline.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrProcessor.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrProcessorAnalysis.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrProcessorSet.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrProgramDesc.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrProgramInfo.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrPromiseImageTexture.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrProxyProvider.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrRecordingContext.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrRecordingContextPriv.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrRenderTarget.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrRenderTargetProxy.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrRenderTask.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrRenderTaskCluster.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrResourceAllocator.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrResourceCache.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrResourceProvider.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrRingBuffer.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrSPIRVUniformHandler.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrSPIRVVaryingHandler.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrSWMaskHelper.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrShaderCaps.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrShaderVar.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrStagingBufferManager.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrStencilSettings.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrStyle.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrSurface.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrSurfaceCharacterization.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrSurfaceProxy.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrSurfaceProxyView.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrTestUtils.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrTexture.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrTextureProxy.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrThreadSafeCache.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrTransferFromRenderTask.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrUniformDataManager.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrUtil.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrVertexChunkArray.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrWaitRenderTask.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrWritePixelsRenderTask.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrXferProcessor.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrYUVABackendTextures.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrYUVATextureProxies.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/PathRenderer.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/PathRendererChain.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/SkGr.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/StencilMaskHelper.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/SurfaceContext.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/SurfaceDrawContext.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/SurfaceFillContext.cpp
    # Ganesh Effects
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrBezierEffect.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrBicubicEffect.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrColorTableEffect.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrCoverageSetOpXP.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrCustomXfermode.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrDisableColorXP.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrMatrixEffect.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrOvalEffect.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrRRectEffect.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrModulateAtlasCoverageEffect.cpp
    # OpenGL 后端
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLUtil.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLProgram.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLInterfaceAutogen.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLGpuProgramCache.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLGpu.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLContext.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLCaps.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLBackendSurface.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLAssembleWebGLInterfaceAutogen.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLAssembleGLInterfaceAutogen.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLAssembleGLESInterfaceAutogen.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/AHardwareBufferGL.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLVertexArray.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLUniformHandler.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLTypesPriv.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLTextureRenderTarget.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLTexture.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLSemaphore.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLRenderTarget.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLProgramDataManager.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLOpsRenderPass.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLMakeNativeInterface_none.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLGLSL.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLFinishCallbacks.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLExtensions.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLDirectContext.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLBuffer.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLAttachment.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLAssembleInterface.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/GrGLAssembleHelpers.cpp
    # GLSL 相关文件
    ${SKIA_SRC_DIR}/gpu/ganesh/glsl/GrGLSLBlend.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/glsl/GrGLSLVarying.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/glsl/GrGLSLVertexGeoBuilder.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/glsl/GrGLSLUniformHandler.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/glsl/GrGLSLProgramDataManager.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/glsl/GrGLSLShaderBuilder.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/glsl/GrGLSLProgramBuilder.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/glsl/GrGLSLFragmentShaderBuilder.cpp
    # 效果相关文件
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrPorterDuffXferProcessor.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrBlendFragmentProcessor.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrTextureEffect.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrDistanceFieldGeoProc.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrBitmapTextGeoProc.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrSkSLFP.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrPerlinNoise2Effect.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrConvexPolyEffect.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrYUVtoRGBEffect.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/builders/GrGLShaderStringBuilder.cpp
    # GPU基础文件
    ${SKIA_SRC_DIR}/gpu/ganesh/GrPipeline.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrProcessorSet.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrOpFlushState.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrMeshDrawTarget.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDistanceFieldGenFromVector.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrFragmentProcessors.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrBackendTextureImageGenerator.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrTextureProxy.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrTextureRenderTargetProxy.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrClientMappedBufferManager.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrBufferTransferRenderTask.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrTextureResolveRenderTask.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/mock/GrMockGpu.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/mock/GrMockCaps.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/mock/GrMockTypes.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/image/GrTextureGenerator.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/image/GrImageUtils.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/image/SkSpecialImage_Ganesh.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/image/SkImage_RasterPinnable.cpp
    ${SKIA_SRC_DIR}/core/SkSpecialImage.cpp
    ${SKIA_SRC_DIR}/core/SkCPURecorder.cpp
    ${SKIA_SRC_DIR}/core/SkCPUContext.cpp
    ${SKIA_SRC_DIR}/core/SkMasks.cpp
    ${SKIA_SRC_DIR}/codec/SkGainmapInfo.cpp
    ${SKIA_SRC_DIR}/codec/SkImageGenerator_FromEncoded.cpp
    ${SKIA_SRC_DIR}/codec/SkEncodedInfo.cpp
    ${SKIA_SRC_DIR}/codec/SkCodec.cpp
    ${SKIA_SRC_DIR}/codec/SkColorPalette.cpp
    ${SKIA_SRC_DIR}/codec/SkSwizzler.cpp
    ${SKIA_SRC_DIR}/codec/SkMaskSwizzler.cpp
    ${SKIA_SRC_DIR}/codec/SkPngCodec.cpp
    ${SKIA_SRC_DIR}/codec/SkPngCodecBase.cpp
    ${SKIA_SRC_DIR}/codec/SkPngCompositeChunkReader.cpp
    ${SKIA_SRC_DIR}/codec/SkBmpCodec.cpp
    ${SKIA_SRC_DIR}/codec/SkBmpBaseCodec.cpp
    ${SKIA_SRC_DIR}/codec/SkBmpMaskCodec.cpp
    ${SKIA_SRC_DIR}/codec/SkBmpRLECodec.cpp
    ${SKIA_SRC_DIR}/codec/SkBmpStandardCodec.cpp
    ${SKIA_SRC_DIR}/codec/SkWbmpCodec.cpp
    ${SKIA_SRC_DIR}/codec/SkCodecImageGenerator.cpp
    ${SKIA_SRC_DIR}/codec/SkPixmapUtils.cpp
    ${SKIA_SRC_DIR}/codec/SkSampler.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/image/SkImage_LazyTexture.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrShadowGeoProc.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gradients/GrGradientShader.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gradients/GrGradientBitmapCache.cpp
    # 路径渲染器
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/TessellationPathRenderer.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/TriangulatingPathRenderer.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/PathTessellateOp.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/StrokeTessellateOp.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/PathStencilCoverOp.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/PathInnerTriangulateOp.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/AtlasPathRenderer.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/AtlasRenderTask.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/DrawAtlasPathOp.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/DashLinePathRenderer.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/DashOp.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/SmallPathRenderer.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/SmallPathAtlasMgr.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/SmallPathShapeData.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/OpsTask.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/AtlasInstancedHelper.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/TextureOp.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/AAConvexPathRenderer.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/AAHairLinePathRenderer.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/AALinearizingConvexPathRenderer.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/DefaultPathRenderer.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/FillRectOp.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/FillRRectOp.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/DrawMeshOp.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/QuadPerEdgeAA.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/RegionOp.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/ShadowRRectOp.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/StrokeRectOp.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/GrOvalOpFactory.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/LatticeOp.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/ClearOp.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/DrawableOp.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/DrawAtlasOp.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/AtlasTextOp.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/SoftwarePathRenderer.cpp
    ${SKIA_SRC_DIR}/text/gpu/VertexFiller.cpp
    ${SKIA_SRC_DIR}/text/gpu/GlyphVector.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/text/GrAtlasManager.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/text/GaneshVertexFiller.cpp
    # 路径渲染器链
    ${SKIA_SRC_DIR}/gpu/ganesh/PathRendererChain.cpp
    # 网格绘制辅助类
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/GrSimpleMeshDrawOpHelper.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/GrSimpleMeshDrawOpHelperWithStencil.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/GrMeshDrawOp.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/ops/GrOp.cpp
    # 几何工具类
    ${SKIA_SRC_DIR}/gpu/ganesh/geometry/GrTriangulator.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/geometry/GrAATriangulator.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/geometry/GrPathUtils.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/geometry/GrStyledShape.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/geometry/GrShape.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/geometry/GrAAConvexTessellator.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/geometry/GrQuadUtils.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/geometry/GrQuad.cpp
    # 曲面细分相关文件
    ${SKIA_SRC_DIR}/gpu/ganesh/tessellate/GrTessellationShader.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/tessellate/GrStrokeTessellationShader.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/tessellate/GrPathTessellationShader.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/tessellate/PathTessellator.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/tessellate/StrokeTessellator.cpp
    # 曲面细分工具
    ${SKIA_SRC_DIR}/gpu/tessellate/FixedCountBufferUtils.cpp
    ${SKIA_SRC_DIR}/gpu/tessellate/Tessellation.cpp
    # Windows OpenGL (WGL) 后端
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/win/GrGLMakeNativeInterface_win.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/win/GrGLMakeWinInterface.cpp
    # Surface相关文件
    ${SKIA_SRC_DIR}/gpu/ganesh/surface/SkSurface_Ganesh.cpp
    # Image相关文件
    ${SKIA_SRC_DIR}/gpu/ganesh/image/SkImage_Ganesh.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/image/SkImage_GaneshBase.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/image/SkImage_GaneshFactories.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/image/SkImage_GaneshYUVA.cpp
    # OpenGL 程序构建器
    ${SKIA_SRC_DIR}/gpu/ganesh/gl/builders/GrGLProgramBuilder.cpp
    # Direct3D 后端（暂时禁用，存在编译错误）
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DTypesMinimal.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DSemaphore.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DResourceProvider.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DPipelineStateDataManager.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DGpu.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DCaps.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DUtil.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DTypesPriv.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DTextureResource.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DTextureRenderTarget.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DTexture.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DRootSignature.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DRenderTarget.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DPipelineStateBuilder.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DPipelineState.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DOpsRenderPass.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DDescriptorTableManager.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DDescriptorHeap.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DCpuDescriptorManager.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DCommandSignature.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DCommandList.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DBuffer.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DAttachment.cpp
    # ${SKIA_SRC_DIR}/gpu/ganesh/d3d/GrD3DAMDMemoryAllocator.cpp
    ${gpu_HEADERS}
)



# skcms 模块
set(skcms_SOURCES
    ${SKIA_ROOT_DIR}/modules/skcms/skcms.cc
    ${SKIA_ROOT_DIR}/modules/skcms/src/skcms_TransformBaseline.cc
    ${SKIA_ROOT_DIR}/modules/skcms/src/skcms_TransformHsw.cc
    ${SKIA_ROOT_DIR}/modules/skcms/src/skcms_TransformSkx.cc
    ${skcms_HEADERS}
)

# 添加源文件分组
source_group(effects FILES ${effects_SOURCES})
source_group(effects\\colorfilters FILES
    ${SKIA_SRC_DIR}/effects/colorfilters/SkBlendModeColorFilter.cpp
    ${SKIA_SRC_DIR}/effects/colorfilters/SkColorFilterBase.cpp
    ${SKIA_SRC_DIR}/effects/colorfilters/SkColorSpaceXformColorFilter.cpp
    ${SKIA_SRC_DIR}/effects/colorfilters/SkComposeColorFilter.cpp
    ${SKIA_SRC_DIR}/effects/colorfilters/SkGaussianColorFilter.cpp
    ${SKIA_SRC_DIR}/effects/colorfilters/SkMatrixColorFilter.cpp
    ${SKIA_SRC_DIR}/effects/colorfilters/SkRuntimeColorFilter.cpp
    ${SKIA_SRC_DIR}/effects/colorfilters/SkTableColorFilter.cpp
    ${SKIA_SRC_DIR}/effects/colorfilters/SkWorkingFormatColorFilter.cpp
)
source_group(effects\\imagefilters FILES
    ${SKIA_SRC_DIR}/effects/imagefilters/SkBlendImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkBlurImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkColorFilterImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkComposeImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkCropImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkDisplacementMapImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkDropShadowImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkImageImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkLightingImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkMagnifierImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkMatrixConvolutionImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkMatrixTransformImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkMergeImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkMorphologyImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkPictureImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkRuntimeImageFilter.cpp
    ${SKIA_SRC_DIR}/effects/imagefilters/SkShaderImageFilter.cpp
)

# 添加源文件分组
source_group(ports FILES ${ports_SOURCES})

# 添加源文件分组
source_group(base FILES ${base_SOURCES})
source_group(core FILES ${core_SOURCES})
source_group(codec FILES ${codec_SOURCES})
source_group(utils FILES ${utils_SOURCES})

# GPU 源文件分组
source_group(gpu FILES
    ${SKIA_SRC_DIR}/gpu/AtlasTypes.cpp
    ${SKIA_SRC_DIR}/gpu/Blend.cpp
    ${SKIA_SRC_DIR}/gpu/BlendFormula.cpp
    ${SKIA_SRC_DIR}/gpu/BlurUtils.cpp
    ${SKIA_SRC_DIR}/gpu/DataUtils.cpp
    ${SKIA_SRC_DIR}/gpu/DitherUtils.cpp
    ${SKIA_SRC_DIR}/gpu/MutableTextureState.cpp
    ${SKIA_SRC_DIR}/gpu/RectanizerPow2.cpp
    ${SKIA_SRC_DIR}/gpu/RectanizerSkyline.cpp
    ${SKIA_SRC_DIR}/gpu/ResourceKey.cpp
    ${SKIA_SRC_DIR}/gpu/ShaderErrorHandler.cpp
    ${SKIA_SRC_DIR}/gpu/SkBackingFit.cpp
    ${SKIA_SRC_DIR}/gpu/SkSLToBackend.cpp
    ${SKIA_SRC_DIR}/gpu/Swizzle.cpp
    ${SKIA_SRC_DIR}/gpu/TiledTextureUtils.cpp
)
source_group(gpu/ganesh FILES
    ${SKIA_SRC_DIR}/gpu/ganesh/ClipStack.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/Device.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/Device_drawTexture.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrAHardwareBufferUtils.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrAttachment.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrAuditTrail.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrBackendSemaphore.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrBackendSurface.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrBackendUtils.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrBlurUtils.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrBufferAllocPool.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrBufferUpdateRenderTask.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrCanvas.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrCaps.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrColorInfo.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrColorSpaceXform.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrContextThreadSafeProxy.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrContext_Base.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrCopyRenderTask.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDDLContext.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDDLTask.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDataUtils.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDefaultGeoProcFactory.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDeferredDisplayList.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDirectContext.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDirectContextPriv.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDrawOpAtlas.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDrawingManager.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDriverBugWorkarounds.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrDynamicAtlas.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrEagerVertexAllocator.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrFixedClip.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrFragmentProcessor.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrFragmentProcessors.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrGeometryProcessor.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrGpu.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrGpuBuffer.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrGpuResource.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrImageContext.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrImageInfo.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrManagedResource.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrMemoryPool.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrMeshBuffers.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrMeshDrawTarget.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrOnFlushResourceProvider.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrOpFlushState.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrOpsRenderPass.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrPaint.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrPersistentCacheUtils.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrPipeline.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrProcessor.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrProcessorAnalysis.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrProcessorSet.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrProgramDesc.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrProgramInfo.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrPromiseImageTexture.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrProxyProvider.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrRecordingContext.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrRecordingContextPriv.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrRenderTarget.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrRenderTargetProxy.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrRenderTask.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrRenderTaskCluster.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrResourceAllocator.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrResourceCache.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrResourceProvider.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrRingBuffer.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrSPIRVUniformHandler.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrSPIRVVaryingHandler.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrSWMaskHelper.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrShaderCaps.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrShaderVar.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrStagingBufferManager.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrStencilSettings.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrStyle.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrSurface.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrSurfaceCharacterization.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrSurfaceProxy.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrSurfaceProxyView.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrTestUtils.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrTexture.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrTextureProxy.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrThreadSafeCache.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrTransferFromRenderTask.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrUniformDataManager.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrUtil.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrVertexChunkArray.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrWaitRenderTask.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrWritePixelsRenderTask.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrXferProcessor.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrYUVABackendTextures.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/GrYUVATextureProxies.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/PathRenderer.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/PathRendererChain.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/SkGr.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/StencilMaskHelper.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/SurfaceContext.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/SurfaceDrawContext.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/SurfaceFillContext.cpp
)
source_group(gpu/ganesh/effects FILES
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrBezierEffect.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrBicubicEffect.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrColorTableEffect.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrCoverageSetOpXP.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrCustomXfermode.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrDisableColorXP.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrMatrixEffect.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrOvalEffect.cpp
    ${SKIA_SRC_DIR}/gpu/ganesh/effects/GrRRectEffect.cpp
)



# skcms 源文件分组
source_group(skcms FILES ${skcms_SOURCES})

# 文本GPU模块
set(text_gpu_SOURCES
    ${SKIA_SRC_DIR}/text/gpu/Slug.cpp
    ${SKIA_SRC_DIR}/text/gpu/SlugImpl.cpp
    ${SKIA_SRC_DIR}/text/gpu/SubRunAllocator.cpp
    ${SKIA_SRC_DIR}/text/gpu/SubRunContainer.cpp
    ${SKIA_SRC_DIR}/text/gpu/SubRunControl.cpp
    ${SKIA_SRC_DIR}/text/gpu/TextBlob.cpp
    ${SKIA_SRC_DIR}/text/gpu/GlyphVector.cpp
    ${SKIA_SRC_DIR}/text/gpu/StrikeCache.cpp
    ${SKIA_SRC_DIR}/text/gpu/SDFMaskFilter.cpp
    ${SKIA_SRC_DIR}/text/gpu/DistanceFieldAdjustTable.cpp
    ${SKIA_SRC_DIR}/text/gpu/SkChromeRemoteGlyphCache.cpp
    ${SKIA_SRC_DIR}/text/gpu/TextBlobRedrawCoordinator.cpp
    ${SKIA_SRC_DIR}/text/gpu/VertexFiller.cpp
)

# 文本模块
set(text_SOURCES
    ${SKIA_SRC_DIR}/text/GlyphRun.cpp
    ${SKIA_SRC_DIR}/text/SlugFromBuffer.cpp
    ${SKIA_SRC_DIR}/text/StrikeForGPU.cpp
)

# Opts模块
set(opts_SOURCES
    ${SKIA_SRC_DIR}/opts/SkOpts_hsw.cpp
    ${SKIA_SRC_DIR}/opts/SkOpts_lasx.cpp
    ${SKIA_SRC_DIR}/opts/SkOpts_skx.cpp
)

# sfnt 模块 - 处理 OpenType 字体表
set(sfnt_SOURCES
    ${SKIA_SRC_DIR}/sfnt/SkOTUtils.cpp
    ${SKIA_SRC_DIR}/sfnt/SkOTTable_name.cpp
)

# 添加 Harfbuzz 头文件路径
include_directories("${SKIA_ROOT_DIR}/third_party/externals/harfbuzz/src")
include_directories("${SKIA_ROOT_DIR}/third_party/harfbuzz")

# 添加 Harfbuzz 源文件
set(HARFBUZZ_SOURCES
    "${SKIA_ROOT_DIR}/third_party/externals/harfbuzz/src/harfbuzz.cc"
)

# 添加 libpng 源文件
set(LIBPNG_SOURCES
    "${SKIA_ROOT_DIR}/third_party/externals/libpng/png.c"
    "${SKIA_ROOT_DIR}/third_party/externals/libpng/pngerror.c"
    "${SKIA_ROOT_DIR}/third_party/externals/libpng/pngget.c"
    "${SKIA_ROOT_DIR}/third_party/externals/libpng/pngmem.c"
    "${SKIA_ROOT_DIR}/third_party/externals/libpng/pngpread.c"
    "${SKIA_ROOT_DIR}/third_party/externals/libpng/pngread.c"
    "${SKIA_ROOT_DIR}/third_party/externals/libpng/pngrio.c"
    "${SKIA_ROOT_DIR}/third_party/externals/libpng/pngrtran.c"
    "${SKIA_ROOT_DIR}/third_party/externals/libpng/pngrutil.c"
    "${SKIA_ROOT_DIR}/third_party/externals/libpng/pngset.c"
    "${SKIA_ROOT_DIR}/third_party/externals/libpng/pngtrans.c"
    "${SKIA_ROOT_DIR}/third_party/externals/libpng/pngwio.c"
    "${SKIA_ROOT_DIR}/third_party/externals/libpng/pngwrite.c"
    "${SKIA_ROOT_DIR}/third_party/externals/libpng/pngwtran.c"
    "${SKIA_ROOT_DIR}/third_party/externals/libpng/pngwutil.c"
)

# 添加 zlib 源文件
set(ZLIB_SOURCES
    "${SKIA_ROOT_DIR}/third_party/externals/zlib/adler32.c"
    "${SKIA_ROOT_DIR}/third_party/externals/zlib/compress.c"
    "${SKIA_ROOT_DIR}/third_party/externals/zlib/crc32.c"
    "${SKIA_ROOT_DIR}/third_party/externals/zlib/deflate.c"
    "${SKIA_ROOT_DIR}/third_party/externals/zlib/gzclose.c"
    "${SKIA_ROOT_DIR}/third_party/externals/zlib/gzlib.c"
    "${SKIA_ROOT_DIR}/third_party/externals/zlib/gzread.c"
    "${SKIA_ROOT_DIR}/third_party/externals/zlib/gzwrite.c"
    "${SKIA_ROOT_DIR}/third_party/externals/zlib/infback.c"
    "${SKIA_ROOT_DIR}/third_party/externals/zlib/inffast.c"
    "${SKIA_ROOT_DIR}/third_party/externals/zlib/inflate.c"
    "${SKIA_ROOT_DIR}/third_party/externals/zlib/inftrees.c"
    "${SKIA_ROOT_DIR}/third_party/externals/zlib/trees.c"
    "${SKIA_ROOT_DIR}/third_party/externals/zlib/uncompr.c"
    "${SKIA_ROOT_DIR}/third_party/externals/zlib/zutil.c"
)

# 添加 SkParagraph、SkShaper、SkUnicode 模块的源文件
set(SKPARAGRAPH_SOURCES
    "${SKIA_ROOT_DIR}/modules/skparagraph/src/Decorations.cpp"
    "${SKIA_ROOT_DIR}/modules/skparagraph/src/FontArguments.cpp"
    "${SKIA_ROOT_DIR}/modules/skparagraph/src/FontCollection.cpp"
    "${SKIA_ROOT_DIR}/modules/skparagraph/src/OneLineShaper.cpp"
    "${SKIA_ROOT_DIR}/modules/skparagraph/src/ParagraphBuilderImpl.cpp"
    "${SKIA_ROOT_DIR}/modules/skparagraph/src/ParagraphCache.cpp"
    "${SKIA_ROOT_DIR}/modules/skparagraph/src/ParagraphImpl.cpp"
    "${SKIA_ROOT_DIR}/modules/skparagraph/src/ParagraphPainterImpl.cpp"
    "${SKIA_ROOT_DIR}/modules/skparagraph/src/ParagraphStyle.cpp"
    "${SKIA_ROOT_DIR}/modules/skparagraph/src/Run.cpp"
    "${SKIA_ROOT_DIR}/modules/skparagraph/src/TextLine.cpp"
    "${SKIA_ROOT_DIR}/modules/skparagraph/src/TextShadow.cpp"
    "${SKIA_ROOT_DIR}/modules/skparagraph/src/TextStyle.cpp"
    "${SKIA_ROOT_DIR}/modules/skparagraph/src/TextWrapper.cpp"
    "${SKIA_ROOT_DIR}/modules/skparagraph/src/TypefaceFontProvider.cpp"
)

set(SKSHAPER_SOURCES
    "${SKIA_ROOT_DIR}/modules/skshaper/src/SkShaper.cpp"
    "${SKIA_ROOT_DIR}/modules/skshaper/src/SkShaper_factory.cpp"
    "${SKIA_ROOT_DIR}/modules/skshaper/src/SkShaper_primitive.cpp"
    "${SKIA_ROOT_DIR}/modules/skshaper/src/SkShaper_harfbuzz.cpp"
    "${SKIA_ROOT_DIR}/modules/skshaper/src/SkShaper_skunicode.cpp"
)

# 使用 GLOB_RECURSE 自动收集 ICU 源文件（仅 .cpp 文件）
file(GLOB_RECURSE ICU_COMMON_SOURCES
    "${SKIA_ROOT_DIR}/third_party/externals/icu/source/common/*.cpp"
)
file(GLOB_RECURSE ICU_I18N_SOURCES
    "${SKIA_ROOT_DIR}/third_party/externals/icu/source/i18n/*.cpp"
)
set(ICU_SOURCES ${ICU_COMMON_SOURCES} ${ICU_I18N_SOURCES})

# 添加 ICU 数据文件（转换后的 C++ 源文件）
set(ICU_DATA_SOURCES
    "${SKIA_ROOT_DIR}/build/icudtl_dat.cpp"
)

# 添加 SkLoadICU.cpp（Windows 平台加载 ICU 数据）
set(ICU_LOAD_SOURCES
    "${SKIA_ROOT_DIR}/third_party/icu/SkLoadICU.cpp"
)

set(SKUNICODE_SOURCES
    "${SKIA_ROOT_DIR}/modules/skunicode/src/SkUnicode.cpp"
    "${SKIA_ROOT_DIR}/modules/skunicode/src/SkUnicode_icu.cpp"
    "${SKIA_ROOT_DIR}/modules/skunicode/src/SkUnicode_icu_bidi.cpp"
    "${SKIA_ROOT_DIR}/modules/skunicode/src/SkUnicode_icu_builtin.cpp"
    "${SKIA_ROOT_DIR}/modules/skunicode/src/SkBidiFactory_icu_full.cpp"
)

# 创建 Skia 库（包含 GPU 渲染模块）
add_library(skia STATIC
    ${base_SOURCES}
    ${core_SOURCES}
    ${utils_SOURCES}
    ${ports_SOURCES}
    ${effects_SOURCES}
    ${gpu_SOURCES}  # 启用 GPU 模块
    ${skcms_SOURCES}
    ${text_SOURCES}
    ${text_gpu_SOURCES}  # 启用 GPU 文本渲染
    ${opts_SOURCES}
    ${sfnt_SOURCES}
    ${shaders_SOURCES}
    ${image_SOURCES}
    ${encode_SOURCES}
    ${sksl_SOURCES}
    ${HARFBUZZ_SOURCES}
    ${SKPARAGRAPH_SOURCES}
    ${SKSHAPER_SOURCES}
    ${SKUNICODE_SOURCES}
    ${ICU_SOURCES}
    ${ICU_DATA_SOURCES}
    ${ICU_LOAD_SOURCES}
    ${LIBPNG_SOURCES}
    ${ZLIB_SOURCES}
)

# 为 ICU 源文件设置特定的宏定义
if(MSVC)
    # ICU common 源文件宏定义
    set_source_files_properties(${ICU_SOURCES} PROPERTIES
        COMPILE_DEFINITIONS "U_COMMON_IMPLEMENTATION;U_STATIC_IMPLEMENTATION;U_ENABLE_DYLOAD=0;U_I18N_IMPLEMENTATION;U_DISABLE_RENAMING=1;U_USING_ICU_NAMESPACE=0;_XOPEN_SOURCE=0;U_NOEXCEPT="
    )
    # ICU 需要 RTTI 支持，所以为 ICU 源文件启用 RTTI
    set_source_files_properties(${ICU_SOURCES} PROPERTIES
        COMPILE_FLAGS "/GR"
    )
endif()

# 为 SkOTTable_name.cpp 禁用重复 case 值警告/错误
if(MSVC)
    set_source_files_properties(${SKIA_SRC_DIR}/sfnt/SkOTTable_name.cpp
        PROPERTIES COMPILE_FLAGS "/wd4065"
    )
endif()

# 设置编译选项
if(MSVC)
    target_compile_options(skia PRIVATE
        /W4
        /EHsc
        /MP
        /GR-
        /GS-
        /Gy
        /Oi
        /Ot
        /wd4267
        /wd4244
        /wd4305
        /wd4800
        /wd4201
        /wd4127
        /wd4100
        /bigobj  # 添加这个选项
    )
    target_compile_definitions(skia PUBLIC 
        SK_DIRECT3D
        SK_GL
    )
    # 链接 OpenGL 和 Direct3D 库
    target_link_libraries(skia PRIVATE opengl32 d3d12 dxgi user32 gdi32 advapi32 shell32 ole32 oleaut32 uuid)
else()
    target_compile_options(skia PRIVATE
        -Wall
        -Wextra
        -fno-exceptions
        -fno-rtti
        -O3
        -Wno-unused-parameter
        -Wno-constant-condition
    )
endif()

# 设置编译定义
target_compile_definitions(skia PRIVATE
    SK_CODEC_DECODES_BMP
    SK_CODEC_DECODES_WBMP
    SK_GAMMA_APPLY_TO_A8
    SK_GPU_ENABLED
    SK_GANESH_ENABLED
    SK_GL
)

# 设置平台特定的编译定义
if(WIN32)
    target_compile_definitions(skia PUBLIC
        SK_BUILD_FOR_WIN32
        SK_WIN
        _CRT_SECURE_NO_WARNINGS
        NOMINMAX
    )
elseif(APPLE)
    target_compile_definitions(skia PRIVATE
        SK_BUILD_FOR_MAC
        SK_MAC
    )
elseif(UNIX)
    target_compile_definitions(skia PRIVATE
        SK_BUILD_FOR_UNIX
        SK_LINUX
        SK_R32_SHIFT=16
    )
endif()

# 设置安装目标
install(TARGETS skia
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)

# 安装头文件
install(DIRECTORY ${SKIA_INCLUDE_DIR}/
    DESTINATION include
)

# 安装 skcms 模块头文件
install(DIRECTORY ${SKIA_ROOT_DIR}/modules/skcms/
    DESTINATION include/modules/skcms
    FILES_MATCHING
    PATTERN "*.h"
)

# 安装 skcms 模块源代码目录（包含 skcms_public.h）
install(DIRECTORY ${SKIA_ROOT_DIR}/modules/skcms/src/
    DESTINATION include/modules/skcms/src
    FILES_MATCHING
    PATTERN "*.h"
)
