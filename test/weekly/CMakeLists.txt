#
# CMAKE file written by Ian Porter, US NRC 11/15/2018
#   This file was adapted from (https://github.com/porteri/vtkmofo) developed
#   by Izaak Beekman, ParaTools.
#
# Weekly Unit tests

function(add_Thermochimica_weekly_unit_test test_name)
  # Assumes source file is ${test_name}.f90
  add_executable(${test_name} ${test_name}.f90)
  target_link_libraries(${test_name} Thermochimica)
  set_property(TARGET ${test_name}
    PROPERTY
    FOLDER "Thermochimica-Weekly-Unit-Tests")
  source_group("Thermochimica\\UnitTests" FILES ${test_name}.f90)
endfunction(add_Thermochimica_weekly_unit_test)

foreach(test
    TestThermo10A
    TestThermo101
    TestThermo102)
  add_Thermochimica_weekly_unit_test(${test})
  list(APPEND Thermochimica_unit_weekly_test_list ${test})
endforeach()

# Set the Thermochimica_weekly_unit_test_list in parent scope
set(Thermochimica_weekly_unit_test_list ${Thermochimica_weekly_unit_test_list} PARENT_SCOPE)
