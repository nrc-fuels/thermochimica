#
# CMAKE file written by Ian Porter, US NRC 11/15/2018
#   This file was adapted from (https://github.com/porteri/vtkmofo) developed
#   by Izaak Beekman, ParaTools.
#
# CMAKE minimum version
cmake_minimum_required(VERSION 3.11)

set(PROJECT_DESCRIPTION "A computational library for chemical thermodynamics")
set(PROJECT_URL "https://github.com/ORNL-CEES/thermochimica")

# Project name
project(Thermochimica LANGUAGES Fortran)

# Turn on the ability to create folders to organize projects (.vcproj)
# It creates "CMakePredefinedTargets" folder by default and adds CMake
# defined projects like INSTALL.vcproj and ZERO_CHECK.vcproj
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

# System options
if ( "${CMAKE_SYSTEM_NAME}" MATCHES "Windows")
  set(prefix "/")
  set(infix ":")
  set(Qf "Q")
  set(Q "Q")
  set(eq ":")
  set(colon ":")
  set(colon_ ":")
  set(libs_static "/libs:static")
  set(dgblibs "/dbglibs")
else()
  set(prefix "-")
  set( infix " ")
  set( Qf "f")
  set( Q "")
  set( eq "=")
  set( colon "")
  set( colon_ " ")
  set( libs_static "")
  set( dgblibs "")
endif()

# Compiler options
if ("${CMAKE_Fortran_COMPILER_ID}" MATCHES "Intel")
  set(intel_compiler true)
  if ( "${CMAKE_SYSTEM_NAME}" MATCHES "Windows")
    # Full debug, Fortran 2008 standard, free format (due to automatic build to .f file for testing), Check bounds
    set(CMAKE_Fortran_FLAGS
      "${prefix}nologo ${prefix}debug${infix}full ${prefix}MP ${prefix}Od ${prefix}standard-semantics ${prefix}warn${infix}errors ${prefix}stand${infix}f15 ${prefix}debug-parameters${infix}all ${prefix}warn${infix}declarations ${prefix}warn${infix}unused ${prefix}warn${infix}interfaces ${prefix}${Qf}trapuv ${prefix}${Q}init${eq}snan ${prefix}${Q}init${eq}arrays ${prefix}fpe${colon}0 ${prefix}traceback ${prefix}check${colon_}bounds ${prefix}check${colon_}stack ${libs_static} ${prefix}threads ${dbglibs} ${prefix}free ${prefix}assume${infix}nounderscore ${prefix}Qmkl:parallel"
       )
  else()
    # Full debug, Fortran 2008 standard, free format (due to automatic build to .f file for testing), Check bounds
    set(CMAKE_Fortran_FLAGS
      "${prefix}nologo ${prefix}debug${infix}full ${prefix}multiple-processes ${prefix}O0 ${prefix}standard-semantics ${prefix}warn${infix}errors ${prefix}stand${infix}f15 ${prefix}debug-parameters${infix}all ${prefix}warn${infix}declarations ${prefix}warn${infix}unused ${prefix}warn${infix}interfaces ${prefix}${Qf}trapuv ${prefix}${Q}init${eq}snan ${prefix}${Q}init${eq}arrays ${prefix}fpe${colon}0 ${prefix}traceback ${prefix}check${colon_}bounds ${prefix}check${colon_}stack ${libs_static} ${prefix}threads ${dbglibs} ${prefix}free ${prefix}assume${infix}nounderscore ${prefix}Qmkl:parallel"
	   )
  endif()
elseif("${CMAKE_Fortran_COMPILER_ID}" MATCHES "GNU")
  set(gnu_compiler true)
  # Fortran 2008 standard, free format (due to automatic build to .f file for testing)
  set(CMAKE_Fortran_FLAGS "${prefix}std=f2008 ${prefix}fcheck=all ${prefix}Wall ${prefix}Wno-unused-dummy-argument ${prefix}ffree-form")
else()
  message(WARNING
    "\n"
    "Attempting to build with untested Fortran compiler: ${CMAKE_Fortran_COMPILER_ID}. "
    "Please report any failures through the thermochimica Git issues\n\n"
  )
endif()

##########################################################
# Begin Thermochimica specific targets and configuration #
##########################################################

# Where to put Thermochimica module files
set(Thermochimica_mod_dir ${CMAKE_CURRENT_BINARY_DIR}/mod)

# Specify Thermochimica sources
set(Thermochimica_files
    AddPureConPhase.f90
    AddSolnPhase.f90
    ArrowSolver.f90
    Broyden.f90
    CheckAddMisciblePhase.f90
    CheckConvergence.f90
    CheckIterHistory.f90
    CheckMiscibilityGap.f90
    CheckPhaseAssemblage.f90
    CheckPhaseAssemblageID.f90
    CheckPhaseChange.f90
    CheckPureConPhaseAdd.f90
    CheckPureConPhaseRem.f90
    CheckPureConPhaseRemHistory.f90
    CheckSolnPhaseAdd.f90
    CheckSolnPhaseRem.f90
    CheckStagnation.f90
    CheckSysOnlyPureConPhases.f90
    CheckSystem.f90
    CheckSystemExcess.f90
    CheckThermoData.f90
    CheckThermoInput.f90
    CompChemicalPotential.f90
    CompDrivingForce.f90
    CompExcessGibbsEnergy.f90
    CompExcessGibbsEnergyQKTO.f90
    CompExcessGibbsEnergyRKMP.f90
    CompExcessGibbsEnergySUBG.f90
    CompExcessGibbsEnergySUBL.f90
    CompFunctionNorm.f90
    CompGibbsMagnetic.f90
    CompGibbsMagneticSoln.f90
    CompMolAllSolnPhases.f90
    CompMolFraction.f90
    CompMolSolnPhase.f90
    CompOtoMRatio.f90
    CompStoichSolnPhase.f90
    CompThermoData.f90
    CorrectMolFraction.f90
    CorrectPhaseRule.f90
    CorrectStagnation.f90
    GEMDebug.f90
    GEMLineSearch.f90
    GEMNewton.f90
    GEMSolver.f90
    GetElementName.f90
    GetFirstAssemblage.f90
    GetNewAssemblage.f90
    GetOutputChemPot.f90
    GetOutputSiteFraction.f90
    GetOutputSolnSpecies.f90
    GetOutputSpeciesStable.f90
    InitGEMSolver.f90
    InitNewPhaseAssemblage.f90
    InitThermo.f90
    KohlerInterpolate.f90
    LevelingSolver.f90
    ModuleGEMSolver.f90
    ModuleParseCS.f90
    ModuleSS.f90
    ModuleSubMin.f90
    ModuleThermo.f90
    ModuleThermoIO.f90
    ParseCSDataBlock.f90
    ParseCSDataBlockGibbs.f90
    ParseCSDataBlockQKTO.f90
    ParseCSDataBlockRKMP.f90
    ParseCSDataBlockSUBG.f90
    ParseCSDataBlockSUBL.f90
    ParseCSDataFile.f90
    ParseCSHeader.f90
    PolyRegularQKTO.f90
    PostLevelingSolver.f90
    PostProcess.f90
    PrintResults.f90
    RemPureConAddSolnPhase.f90
    RemPureConPhase.f90
    RemSolnAddPureConPhase.f90
    RemSolnPhase.f90
    ResetThermo.f90
    ResetThermoAll.f90
    ResetThermoParser.f90
    RevertSystem.f90
    ShuffleAssemblage.f90
    SortPick.f90
    Subminimization.f90
    SwapPureConForSolnPhase.f90
    SwapPureConPhase.f90
    SwapSolnForPureConPhase.f90
    SwapSolnPhase.f90
    SwapSolnPhaseSpecific.f90
    Thermochimica.f90
    ThermoDebug.f90
    ThermoOutput.f90
  )
foreach(item ${Thermochimica_files})
  list(APPEND Thermochimica_sources "${CMAKE_CURRENT_SOURCE_DIR}/src/${item}")
endforeach()

# Make Thermochimica static library
add_library(Thermochimica STATIC ${Thermochimica_sources})

# Tell CMake where to put Thermochimica .mod files generated with libThermochimica
set_property(TARGET Thermochimica
  PROPERTY
  Fortran_MODULE_DIRECTORY ${Thermochimica_mod_dir})

# Tell consumers where to find .mod files
target_include_directories(Thermochimica PUBLIC
  $<BUILD_INTERFACE:${Thermochimica_mod_dir}>
  $<INSTALL_INTERFACE:include/Thermochimica/mod>
  )

# Organize things in Visual Studio
source_group("ThermochimicaLib" FILES ${Thermochimica_sources})
set_property(TARGET Thermochimica
  PROPERTY
  FOLDER "Thermochimica")
install(TARGETS Thermochimica DESTINATION lib EXPORT Thermochimica-targets)
install(EXPORT Thermochimica-targets DESTINATION lib/Thermochimica)
install(DIRECTORY "${Thermochimica_mod_dir}" DESTINATION include/Thermochimica)

######################
# Add test directories
######################

enable_testing()

# Specify all the subdirectories containing test executable/library sources:
set(subdirectories_list
  test/daily
  test/weekly
  )
foreach(subdirectory ${subdirectories_list})
  add_subdirectory("${CMAKE_CURRENT_SOURCE_DIR}/${subdirectory}")
endforeach()

# Unit Tests exported from test/daily subdirectory in ${Thermochimica_daily_unit_test_list}
# Unit Tests exported from test/daily subdirectory in ${Thermochimica_weekly_unit_test_list}

# Add DAILY unit tests and define the string that is used to signal success
foreach(unit_test ${Thermochimica_daily_unit_test_list})
  add_test(NAME "Thermochimica_daily_${unit_test}_test" COMMAND ${CMAKE_CURRENT_BINARY_DIR}/test/daily/${unit_test} WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/test/daily)
  set_property(TEST "Thermochimica_daily_${unit_test}_test" PROPERTY PASS_REGULAR_EXPRESSION "PASS")
  set_property(TEST "Thermochimica_daily_${unit_test}_test" PROPERTY LABELS "Thermochimica" "daily-unit-test")
endforeach()

# Add WEEKLY unit tests and define the string that is used to signal success
foreach(unit_test ${Thermochimica_weekly_unit_test_list})
  add_test(NAME "Thermochimica_weekly_${unit_test}_test" COMMAND ${CMAKE_CURRENT_BINARY_DIR}/test/weekly/${unit_test} WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/test/weekly)
  set_property(TEST "Thermochimica_weekly_${unit_test}_test" PROPERTY PASS_REGULAR_EXPRESSION "PASS")
  set_property(TEST "Thermochimica_weekly_${unit_test}_test" PROPERTY LABELS "Thermochimica" "weekly-unit-test")
endforeach()

add_custom_target(RUN_ALL_Thermochimica_TESTS
  COMMAND ${CMAKE_CTEST_COMMAND} -C $<CONFIG> --output-on-failure -L "Thermochimica")
set_property(TARGET RUN_ALL_Thermochimica_TESTS PROPERTY FOLDER "All-Tests")
