###############################################################################
#
# test/unit/CMakeLists.txt controls building of PDAL unit tests suite
#
# Copyright (c) 2009 Mateusz Loskot <mateusz@loskot.net>
#
###############################################################################

configure_file(TestConfig.hpp.in "${CMAKE_CURRENT_BINARY_DIR}/TestConfig.hpp")

file(GLOB_RECURSE inFiles RELATIVE "${CMAKE_SOURCE_DIR}" "${CMAKE_SOURCE_DIR}/test/data/*.in")
foreach(infileName ${inFiles})
    string(REGEX REPLACE ".in\$" "" outfileName "${infileName}")
    configure_file("${CMAKE_SOURCE_DIR}/${infileName}"
        "${CMAKE_BINARY_DIR}/${outfileName}")
endforeach()

add_library(${PDAL_TEST_SUPPORT_OBJS} OBJECT Support.cpp)
add_dependencies(${PDAL_TEST_SUPPORT_OBJS} generate_dimension_hpp)
target_include_directories(${PDAL_TEST_SUPPORT_OBJS}
    PRIVATE
        ${ROOT_DIR}
        ${PROJECT_BINARY_DIR}/include
        ${PROJECT_BINARY_DIR}/test/unit
        ${PDAL_INCLUDE_DIR}
)
PDAL_TARGET_COMPILE_SETTINGS(${PDAL_TEST_SUPPORT_OBJS})

PDAL_ADD_TEST(pdal_bounds_test FILES BoundsTest.cpp)
PDAL_ADD_TEST(pdal_config_test FILES ConfigTest.cpp)
PDAL_ADD_TEST(pdal_eigen_test FILES EigenTest.cpp)
target_include_directories(pdal_eigen_test PRIVATE ${PDAL_VENDOR_DIR}/eigen)
PDAL_ADD_TEST(pdal_file_utils_test FILES FileUtilsTest.cpp)
PDAL_ADD_TEST(pdal_georeference_test FILES GeoreferenceTest.cpp)
PDAL_ADD_TEST(pdal_kdindex_test FILES KDIndexTest.cpp)
target_include_directories(pdal_kdindex_test PRIVATE ${PDAL_VENDOR_DIR})
PDAL_ADD_TEST(pdal_kernel_test FILES KernelTest.cpp)
PDAL_ADD_TEST(pdal_log_test FILES LogTest.cpp)
PDAL_ADD_TEST(pdal_metadata_test FILES MetadataTest.cpp)
PDAL_ADD_TEST(pdal_oldpclblock_test FILES OldPCLBlockTest.cpp)
PDAL_ADD_TEST(pdal_options_test
    FILES
        OptionsTest.cpp
    LINK_WITH
        ${PDAL_JSONCPP_LIB_NAME}
        ${GDAL_LIBRARY}
)
    target_include_directories(pdal_options_test PRIVATE
        ${PDAL_JSONCPP_INCLUDE_DIR})
PDAL_ADD_TEST(pdal_pipeline_manager_test FILES PipelineManagerTest.cpp)
PDAL_ADD_TEST(pdal_plugin_manager_test FILES PluginManagerTest.cpp)
PDAL_ADD_TEST(pdal_point_view_test FILES PointViewTest.cpp)
PDAL_ADD_TEST(pdal_point_table_test FILES PointTableTest.cpp)
PDAL_ADD_TEST(pdal_program_arg_test
    FILES
        ProgramArgsTest.cpp
    LINK_WITH
        ${PDAL_JSONCPP_LIB_NAME}
)
    target_include_directories(pdal_program_arg_test PRIVATE
        ${PDAL_JSONCPP_INCLUDE_DIR})
PDAL_ADD_TEST(pdal_artifact_test FILES ArtifactTest.cpp)

PDAL_ADD_TEST(pdal_polygon_test
    FILES
        PolygonTest.cpp
    LINK_WITH
        ${GDAL_LIBRARY}
)
PDAL_ADD_TEST(pdal_segmentation_test FILES SegmentationTest.cpp)
PDAL_ADD_TEST(pdal_spatial_reference_test
    FILES
        SpatialReferenceTest.cpp
    LINK_WITH
        ${GDAL_LIBRARY}
)
target_include_directories(pdal_spatial_reference_test PRIVATE ${PDAL_JSONCPP_INCLUDE_DIR})
PDAL_ADD_TEST(pdal_stage_factory_test FILES StageFactoryTest.cpp)
PDAL_ADD_TEST(pdal_streaming_test FILES StreamingTest.cpp)
PDAL_ADD_TEST(pdal_support_test FILES SupportTest.cpp)
PDAL_ADD_TEST(pdal_utils_test FILES UtilsTest.cpp)
PDAL_ADD_TEST(pdal_uuid_test FILES UuidTest.cpp)
if (PDAL_HAVE_LAZ_PERF)
PDAL_ADD_TEST(pdal_lazperf_test FILES LazPerfTest.cpp)
endif()
if (PDAL_HAVE_ZLIB)
PDAL_ADD_TEST(pdal_deflate_test FILES DeflateTest.cpp)
endif()
if (PDAL_HAVE_LZMA)
PDAL_ADD_TEST(pdal_lzma_test FILES LzmaTest.cpp)
endif()
if (PDAL_HAVE_ZSTD)
PDAL_ADD_TEST(pdal_zstd_test FILES ZstdTest.cpp)
endif()

#
# sources for the native io
#
PDAL_ADD_TEST(pdal_io_bpf_base_test FILES io/BpfTestBase.cpp)
if (PDAL_HAVE_ZLIB)
    PDAL_ADD_TEST(pdal_io_bpf_zlib_test FILES io/BpfTestZlib.cpp)
endif()
PDAL_ADD_TEST(pdal_io_buffer_test FILES io/BufferTest.cpp)
PDAL_ADD_TEST(pdal_io_ept_reader_test
    FILES
        io/EptReaderTest.cpp
    LINK_WITH
        ${GDAL_LIBRARY} # EptReaderTest uses the crop filter.
)

target_include_directories(pdal_io_ept_reader_test PRIVATE ${PDAL_JSONCPP_INCLUDE_DIR})
PDAL_ADD_TEST(pdal_io_faux_test FILES io/FauxReaderTest.cpp)
PDAL_ADD_TEST(pdal_io_gdal_reader_test
    FILES
        io/GDALReaderTest.cpp
    LINK_WITH
        ${GDAL_LIBRARY}
)
PDAL_ADD_TEST(pdal_io_gdal_writer_test
    FILES
        io/GDALWriterTest.cpp
    LINK_WITH
        ${GDAL_LIBRARY}
)
PDAL_ADD_TEST(pdal_io_las_reader_test FILES io/LasReaderTest.cpp)
target_include_directories(pdal_io_las_reader_test PRIVATE ${PDAL_JSONCPP_INCLUDE_DIR})
PDAL_ADD_TEST(pdal_io_las_writer_test FILES io/LasWriterTest.cpp)
target_include_directories(pdal_io_las_writer_test PRIVATE ${PDAL_JSONCPP_INCLUDE_DIR})
PDAL_ADD_TEST(pdal_io_optech_test FILES io/OptechReaderTest.cpp)
PDAL_ADD_TEST(pdal_io_ply_reader_test FILES io/PlyReaderTest.cpp)
target_include_directories(pdal_io_ply_reader_test PRIVATE ${PDAL_VENDOR_DIR})
PDAL_ADD_TEST(pdal_io_ply_writer_test FILES io/PlyWriterTest.cpp)
target_include_directories(pdal_io_ply_writer_test PRIVATE ${PDAL_VENDOR_DIR})
PDAL_ADD_TEST(pdal_io_pts_reader_test FILES io/PtsReaderTest.cpp)
PDAL_ADD_TEST(pdal_io_qfit_test FILES io/QFITReaderTest.cpp)
PDAL_ADD_TEST(pdal_io_sbet_reader_test FILES io/SbetReaderTest.cpp)
PDAL_ADD_TEST(pdal_io_sbet_writer_test FILES io/SbetWriterTest.cpp)
PDAL_ADD_TEST(pdal_io_terrasolid_test FILES io/TerrasolidReaderTest.cpp)
PDAL_ADD_TEST(pdal_io_text_reader_test FILES io/TextReaderTest.cpp)
target_include_directories(pdal_io_text_reader_test PRIVATE ${PDAL_JSONCPP_INCLUDE_DIR})
PDAL_ADD_TEST(pdal_io_text_writer_test FILES io/TextWriterTest.cpp)

#
# sources for the native filters
#
PDAL_ADD_TEST(pdal_filters_assign_test FILES filters/AssignFilterTest.cpp)
PDAL_ADD_TEST(pdal_filters_chipper_test FILES filters/ChipperTest.cpp)
PDAL_ADD_TEST(pdal_filters_nndistance_test FILES filters/NNDistanceTest.cpp)
target_include_directories(pdal_filters_chipper_test PRIVATE ${PDAL_JSONCPP_INCLUDE_DIR})
PDAL_ADD_TEST(pdal_filters_colorinterp_test
    FILES
        filters/ColorinterpFilterTest.cpp
    LINK_WITH
        ${GDAL_LIBRARY}
)
PDAL_ADD_TEST(pdal_filters_colorization_test
    FILES
        filters/ColorizationFilterTest.cpp
    LINK_WITH
        ${GDAL_LIBRARY}
)
PDAL_ADD_TEST(pdal_filters_crop_test
    FILES
        filters/CropFilterTest.cpp
    LINK_WITH
        ${GDAL_LIBRARY}
)
PDAL_ADD_TEST(pdal_filters_decimation_test FILES
    filters/DecimationFilterTest.cpp)
PDAL_ADD_TEST(pdal_filters_divider_test FILES filters/DividerFilterTest.cpp)
PDAL_ADD_TEST(pdal_filters_ferry_test FILES filters/FerryFilterTest.cpp)
PDAL_ADD_TEST(pdal_filters_groupby_test FILES filters/GroupByFilterTest.cpp)
PDAL_ADD_TEST(pdal_filters_info_test FILES filters/InfoFilterTest.cpp)
PDAL_ADD_TEST(pdal_filters_neighborclassifier_test FILES filters/NeighborClassifierFilterTest.cpp)
PDAL_ADD_TEST(pdal_filters_locate_test FILES filters/LocateFilterTest.cpp)
PDAL_ADD_TEST(pdal_filters_merge_test FILES filters/MergeTest.cpp)
PDAL_ADD_TEST(pdal_morton_order_test FILES filters/MortonOrderTest.cpp)
PDAL_ADD_TEST(pdal_filters_additional_merge_test FILES
    filters/AdditionalMergeTest.cpp)
target_include_directories(pdal_filters_additional_merge_test PRIVATE ${PDAL_JSONCPP_INCLUDE_DIR})
PDAL_ADD_TEST(pdal_filters_overlay_test FILES filters/OverlayFilterTest.cpp)
PDAL_ADD_TEST(pdal_filters_pmf_test FILES filters/PMFFilterTest.cpp)
PDAL_ADD_TEST(pdal_filters_reprojection_test FILES
    filters/ReprojectionFilterTest.cpp)
PDAL_ADD_TEST(pdal_filters_range_test FILES filters/RangeFilterTest.cpp)
PDAL_ADD_TEST(pdal_filters_randomize_test FILES filters/RandomizeFilterTest.cpp)
PDAL_ADD_TEST(pdal_filters_returns_test FILES filters/ReturnsFilterTest.cpp)
PDAL_ADD_TEST(pdal_filters_smrf_test FILES filters/SMRFilterTest.cpp)
PDAL_ADD_TEST(pdal_filters_sort_test FILES filters/SortFilterTest.cpp)
target_include_directories(pdal_filters_sort_test PRIVATE ${PDAL_JSONCPP_INCLUDE_DIR})
PDAL_ADD_TEST(pdal_filters_splitter_test FILES filters/SplitterTest.cpp)
PDAL_ADD_TEST(pdal_filters_stats_test FILES filters/StatsFilterTest.cpp)
PDAL_ADD_TEST(pdal_filters_transformation_test FILES
    filters/TransformationFilterTest.cpp)
PDAL_ADD_TEST(pdal_filters_hexbin_test FILES filters/HexbinFilterTest.cpp)
PDAL_ADD_TEST(pdal_filters_voxel_test FILES filters/VoxelTest.cpp)

PDAL_ADD_TEST(pdal_app_test FILES apps/AppTest.cpp)
PDAL_ADD_TEST(pdal_app_plugin_test FILES apps/AppPluginTest.cpp)
PDAL_ADD_TEST(pdal_info_test FILES apps/InfoTest.cpp)
PDAL_ADD_TEST(pdal_tile_test FILES apps/TileTest.cpp)
PDAL_ADD_TEST(pdal_tindex_test FILES apps/TIndexTest.cpp)
if (LASZIP_FOUND)
    PDAL_ADD_TEST(pdal_merge_test FILES apps/MergeTest.cpp)
endif()
PDAL_ADD_TEST(pc2pc_test FILES apps/pc2pcTest.cpp)

if (BUILD_PIPELINE_TESTS)
    PDAL_ADD_TEST(pcpipeline_test_json FILES apps/pcpipelineTestJSON.cpp)
endif()
PDAL_ADD_TEST(hausdorff_test FILES apps/HausdorffTest.cpp)
PDAL_ADD_TEST(random_test FILES apps/RandomTest.cpp)
PDAL_ADD_TEST(translate_test FILES apps/TranslateTest.cpp)

if(PDAL_HAVE_LIBXML2)
    PDAL_ADD_TEST(pdal_io_ilvis2_metadata_test FILES
        io/Ilvis2MetadataReaderTest.cpp)
    target_include_directories(pdal_io_ilvis2_metadata_test PRIVATE
        ${LIBXML2_INCLUDE_DIR})
    PDAL_ADD_TEST(pdal_io_ilvis2_reader_metadata_test FILES
        io/Ilvis2ReaderWithMDReaderTest.cpp)
    target_include_directories(pdal_io_ilvis2_reader_metadata_test PRIVATE
        ${LIBXML2_INCLUDE_DIR})
    PDAL_ADD_TEST(xml_schema_test
        FILES
            XMLSchemaTest.cpp
        LINK_WITH
            ${LIBXML2_LIBRARIES}
    )
    target_include_directories(xml_schema_test PRIVATE
        ${LIBXML2_INCLUDE_DIR})
    PDAL_ADD_TEST(pdal_io_ilvis2_test FILES io/Ilvis2ReaderTest.cpp)
    target_include_directories(pdal_io_ilvis2_test PRIVATE
        ${LIBXML2_INCLUDE_DIR})
endif()
