#
# CMAKE file written by Ian Porter, US NRC 11/15/2018
#   This file was adapted from (https://github.com/porteri/vtkmofo) developed
#   by Izaak Beekman, ParaTools.
#
# Daily Unit tests

function(add_Thermochimica_daily_unit_test test_name)
  # Assumes source file is ${test_name}.f90
  add_executable(${test_name} ${test_name}.f90)# $<TARGET_OBJECTS:ThermochimicaPassFail>)
  target_link_libraries(${test_name} Thermochimica)
  set_property(TARGET ${test_name}
    PROPERTY
    FOLDER "Thermochimica-Daily-Unit-Tests")
  source_group("src" FILES ${test_name}.f90)
endfunction(add_Thermochimica_daily_unit_test)

foreach(test
    TestThermo01
    TestThermo02
    TestThermo03
    TestThermo04
    TestThermo05
    TestThermo06
    TestThermo07
    TestThermo08
    TestThermo09
    TestThermo10
    TestThermo11
    TestThermo12
    TestThermo13
    TestThermo30
    TestThermo31
    TestThermo32
    TestThermo33
    TestThermo40
    TestThermo41
    TestThermo42
    TestThermo43
    TestThermo44
    TestThermo45
    TestThermo46
    TestThermo47
    TestThermo48)
  add_Thermochimica_daily_unit_test(${test})
  list(APPEND Thermochimica_daily_unit_test_list ${test})
endforeach()

# Set the Thermochimica_daily_unit_test_list in parent scope
set(Thermochimica_daily_unit_test_list ${Thermochimica_daily_unit_test_list} PARENT_SCOPE)
