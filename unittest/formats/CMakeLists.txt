
add_executable(test_atom_styles test_atom_styles.cpp)
target_link_libraries(test_atom_styles PRIVATE lammps GTest::GMock)
add_test(NAME AtomStyles COMMAND test_atom_styles WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})

add_executable(test_image_flags test_image_flags.cpp)
target_link_libraries(test_image_flags PRIVATE lammps GTest::GMock)
add_test(NAME ImageFlags COMMAND test_image_flags WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})

add_executable(test_input_convert test_input_convert.cpp)
target_link_libraries(test_input_convert PRIVATE lammps GTest::GMockMain)
add_test(NAME InputConvert COMMAND test_input_convert WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})

add_executable(test_molecule_file test_molecule_file.cpp)
target_link_libraries(test_molecule_file PRIVATE lammps GTest::GMock)
add_test(NAME MoleculeFile COMMAND test_molecule_file WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})

add_executable(test_pair_unit_convert test_pair_unit_convert.cpp)
target_link_libraries(test_pair_unit_convert PRIVATE lammps GTest::GMock)
add_test(NAME PairUnitConvert COMMAND test_pair_unit_convert WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
set_tests_properties(PairUnitConvert PROPERTIES ENVIRONMENT "LAMMPS_POTENTIALS=${LAMMPS_POTENTIALS_DIR}")

add_executable(test_potential_file_reader test_potential_file_reader.cpp)
target_link_libraries(test_potential_file_reader PRIVATE lammps GTest::GMock)
add_test(NAME PotentialFileReader COMMAND test_potential_file_reader WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
set_tests_properties(PotentialFileReader PROPERTIES ENVIRONMENT "LAMMPS_POTENTIALS=${LAMMPS_POTENTIALS_DIR}")

if(PKG_MANYBODY)
    add_executable(test_eim_potential_file_reader test_eim_potential_file_reader.cpp)
    target_link_libraries(test_eim_potential_file_reader PRIVATE lammps GTest::GMock)
    add_test(NAME EIMPotentialFileReader COMMAND test_eim_potential_file_reader WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
    set_tests_properties(EIMPotentialFileReader PROPERTIES ENVIRONMENT "LAMMPS_POTENTIALS=${LAMMPS_POTENTIALS_DIR}")
endif()

add_executable(test_text_file_reader test_text_file_reader.cpp)
target_link_libraries(test_text_file_reader PRIVATE lammps GTest::GMock)
add_test(NAME TextFileReader COMMAND test_text_file_reader WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
set_tests_properties(TextFileReader PROPERTIES ENVIRONMENT "LAMMPS_POTENTIALS=${LAMMPS_POTENTIALS_DIR}")

add_executable(test_file_operations test_file_operations.cpp)
target_link_libraries(test_file_operations PRIVATE lammps GTest::GMock)
add_test(NAME FileOperations COMMAND test_file_operations WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})

add_executable(test_dump_atom test_dump_atom.cpp)
target_link_libraries(test_dump_atom PRIVATE lammps GTest::GMock)
add_test(NAME DumpAtom COMMAND test_dump_atom WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
set_tests_properties(DumpAtom PROPERTIES ENVIRONMENT "LAMMPS_POTENTIALS=${LAMMPS_POTENTIALS_DIR}")

if(PKG_COMPRESS)
    find_program(GZIP_BINARY NAMES gzip REQUIRED)

    add_executable(test_dump_atom_compressed test_dump_atom_compressed.cpp compressed_dump_test_main.cpp)
    target_link_libraries(test_dump_atom_compressed PRIVATE lammps GTest::GMock)

    add_executable(test_dump_custom_compressed test_dump_custom_compressed.cpp compressed_dump_test_main.cpp)
    target_link_libraries(test_dump_custom_compressed PRIVATE lammps GTest::GMock)

    add_executable(test_dump_cfg_compressed test_dump_cfg_compressed.cpp compressed_dump_test_main.cpp)
    target_link_libraries(test_dump_cfg_compressed PRIVATE lammps GTest::GMock)

    add_executable(test_dump_local_compressed test_dump_local_compressed.cpp compressed_dump_test_main.cpp)
    target_link_libraries(test_dump_local_compressed PRIVATE lammps GTest::GMock)

    add_executable(test_dump_xyz_compressed test_dump_xyz_compressed.cpp compressed_dump_test_main.cpp)
    target_link_libraries(test_dump_xyz_compressed PRIVATE lammps GTest::GMock)

    add_test(NAME DumpAtomGZ COMMAND test_dump_atom_compressed gz WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
    set_tests_properties(DumpAtomGZ PROPERTIES ENVIRONMENT "LAMMPS_POTENTIALS=${LAMMPS_POTENTIALS_DIR};COMPRESS_BINARY=${GZIP_BINARY}")

    add_test(NAME DumpCustomGZ COMMAND test_dump_custom_compressed gz WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
    set_tests_properties(DumpCustomGZ PROPERTIES ENVIRONMENT "LAMMPS_POTENTIALS=${LAMMPS_POTENTIALS_DIR};COMPRESS_BINARY=${GZIP_BINARY}")

    add_test(NAME DumpCfgGZ COMMAND test_dump_cfg_compressed gz WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
    set_tests_properties(DumpCfgGZ PROPERTIES ENVIRONMENT "LAMMPS_POTENTIALS=${LAMMPS_POTENTIALS_DIR};COMPRESS_BINARY=${GZIP_BINARY}")

    add_test(NAME DumpLocalGZ COMMAND test_dump_local_compressed gz WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
    set_tests_properties(DumpLocalGZ PROPERTIES ENVIRONMENT "LAMMPS_POTENTIALS=${LAMMPS_POTENTIALS_DIR};COMPRESS_BINARY=${GZIP_BINARY}")

    add_test(NAME DumpXYZGZ COMMAND test_dump_xyz_compressed gz WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
    set_tests_properties(DumpXYZGZ PROPERTIES ENVIRONMENT "LAMMPS_POTENTIALS=${LAMMPS_POTENTIALS_DIR};COMPRESS_BINARY=${GZIP_BINARY}")

    find_package(PkgConfig REQUIRED)
    pkg_check_modules(Zstd IMPORTED_TARGET libzstd>=1.4)
    find_program(ZSTD_BINARY NAMES zstd)

    if(Zstd_FOUND AND ZSTD_BINARY)
        add_test(NAME DumpAtomZstd COMMAND test_dump_atom_compressed zstd WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
        set_tests_properties(DumpAtomZstd PROPERTIES ENVIRONMENT "LAMMPS_POTENTIALS=${LAMMPS_POTENTIALS_DIR};COMPRESS_BINARY=${ZSTD_BINARY}")

        add_test(NAME DumpCustomZstd COMMAND test_dump_custom_compressed zstd WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
        set_tests_properties(DumpCustomZstd PROPERTIES ENVIRONMENT "LAMMPS_POTENTIALS=${LAMMPS_POTENTIALS_DIR};COMPRESS_BINARY=${ZSTD_BINARY}")

        add_test(NAME DumpCfgZstd COMMAND test_dump_cfg_compressed zstd WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
        set_tests_properties(DumpCfgZstd PROPERTIES ENVIRONMENT "LAMMPS_POTENTIALS=${LAMMPS_POTENTIALS_DIR};COMPRESS_BINARY=${ZSTD_BINARY}")

        add_test(NAME DumpLocalZstd COMMAND test_dump_local_compressed zstd WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
        set_tests_properties(DumpLocalZstd PROPERTIES ENVIRONMENT "LAMMPS_POTENTIALS=${LAMMPS_POTENTIALS_DIR};COMPRESS_BINARY=${ZSTD_BINARY}")

        add_test(NAME DumpXYZZstd COMMAND test_dump_xyz_compressed zstd WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
        set_tests_properties(DumpXYZZstd PROPERTIES ENVIRONMENT "LAMMPS_POTENTIALS=${LAMMPS_POTENTIALS_DIR};COMPRESS_BINARY=${ZSTD_BINARY}")
    endif()
endif()

add_executable(test_dump_custom test_dump_custom.cpp)
target_link_libraries(test_dump_custom PRIVATE lammps GTest::GMock)
add_test(NAME DumpCustom COMMAND test_dump_custom WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
set_tests_properties(DumpCustom PROPERTIES ENVIRONMENT "LAMMPS_POTENTIALS=${LAMMPS_POTENTIALS_DIR}")

add_executable(test_dump_cfg test_dump_cfg.cpp)
target_link_libraries(test_dump_cfg PRIVATE lammps GTest::GMock)
add_test(NAME DumpCfg COMMAND test_dump_cfg WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
set_tests_properties(DumpCfg PROPERTIES ENVIRONMENT "LAMMPS_POTENTIALS=${LAMMPS_POTENTIALS_DIR}")

add_executable(test_dump_local test_dump_local.cpp)
target_link_libraries(test_dump_local PRIVATE lammps GTest::GMock)
add_test(NAME DumpLocal COMMAND test_dump_local WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
set_tests_properties(DumpLocal PROPERTIES ENVIRONMENT "LAMMPS_POTENTIALS=${LAMMPS_POTENTIALS_DIR}")

if(PKG_NETCDF)
  find_program(NCDUMP NAMES ncdump ncdump.exe)
  add_executable(test_dump_netcdf test_dump_netcdf.cpp)
  target_link_libraries(test_dump_netcdf PRIVATE lammps GTest::GMock)
  add_test(NAME DumpNetCDF COMMAND test_dump_netcdf WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
  if(NOT (NCDUMP STREQUAL "NCDUMP-NOTFOUND"))
    set_tests_properties(DumpNetCDF PROPERTIES ENVIRONMENT "LAMMPS_POTENTIALS=${LAMMPS_POTENTIALS_DIR};NCDUMP_BINARY=${NCDUMP}")
  endif()
endif()

if(BUILD_TOOLS)
    set_tests_properties(DumpAtom PROPERTIES ENVIRONMENT "BINARY2TXT_BINARY=$<TARGET_FILE:binary2txt>")
    set_tests_properties(DumpCustom PROPERTIES ENVIRONMENT "BINARY2TXT_BINARY=$<TARGET_FILE:binary2txt>")
endif()
