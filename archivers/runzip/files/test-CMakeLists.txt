add_test(NAME ZipFixTest
         COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/check-runzip.sh
         WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)
set_property(TEST ZipFixTest 
             PROPERTY ENVIRONMENT "RUNZIP=../build/runzip") 