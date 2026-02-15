cmake_minimum_required(VERSION 3.16 FATAL_ERROR)
cmake_policy(VERSION 3.16)
project(Skia LANGUAGES C CXX)

# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# 设置 CMake 变量
set(SKIA_ROOT_DIR ${CMAKE_CURRENT_SOURCE_DIR})
set(SKIA_SRC_DIR ${SKIA_ROOT_DIR}/src)
set(SKIA_INCLUDE_DIR ${SKIA_ROOT_DIR}/include)

# 添加 include 目录
include_directories(${SKIA_ROOT_DIR})
include_directories(${SKIA_INCLUDE_DIR})

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
)

# 核心模块
set(core_SOURCES
    ${SKIA_SRC_DIR}/core/SkAAClip.cpp
    ${SKIA_SRC_DIR}/core/SkATrace.cpp
    ${SKIA_SRC_DIR}/core/SkAlphaRuns.cpp
    ${SKIA_SRC_DIR}/core/SkAnnotation.cpp
    ${SKIA_SRC_DIR}/core/SkBBHFactory.cpp
    ${SKIA_SRC_DIR}/core/SkBigPicture.cpp
    ${SKIA_SRC_DIR}/core/SkBitmap.cpp
    ${SKIA_SRC_DIR}/core/SkBitmapCache.cpp
    ${SKIA_SRC_DIR}/core/SkBitmapDevice.cpp
    ${SKIA_SRC_DIR}/core/SkBlendMode.cpp
    ${SKIA_SRC_DIR}/core/SkBlitter.cpp
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
    ${SKIA_SRC_DIR}/core/SkStroke.cpp
    ${SKIA_SRC_DIR}/core/SkStrokeRec.cpp
    ${SKIA_SRC_DIR}/core/SkStrokerPriv.cpp
    ${SKIA_SRC_DIR}/core/SkSwizzle.cpp
    ${SKIA_SRC_DIR}/core/SkWriteBuffer.cpp
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

# 创建 Skia 库
add_library(skia STATIC
    ${base_SOURCES}
    ${core_SOURCES}
    ${codec_SOURCES}
    ${utils_SOURCES}
    ${ports_SOURCES}
    ${effects_SOURCES}
)

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
    )
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
)

# 设置平台特定的编译定义
if(WIN32)
    target_compile_definitions(skia PRIVATE
        SK_BUILD_FOR_WIN32
        SK_WIN
        _CRT_SECURE_NO_WARNINGS
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
    DESTINATION include/skia
)
