include(FetchContent)
FetchContent_Declare(axi_chi 
    GIT_REPOSITORY https://github.com/Arteris-IP/tlm2-interfaces.git
    GIT_TAG d564224
    )
FetchContent_Declare(lwtr4sc
    GIT_REPOSITORY https://github.com/Minres/LWTR4SC.git
    GIT_TAG 04c288d
    SOURCE_SUBDIR this-directory-does-not-exist
    )

add_subdirectory(fst)
if(NOT RapidJSON_FOUND)
    add_subdirectory(rapidjson-1.1)
endif()
###############################################################################
# build tlm-interfaces
###############################################################################
if(SystemC_FOUND)
	option(USE_CCI_100 "" OFF)
    FetchContent_MakeAvailable(lwtr4sc axi_chi)
	add_subdirectory(scv-tr)
    if(NOT TARGET systemc-cci::systemc-cci AND NOT TARGET SystemC::cci)
    	if(USE_CCI_100)
        	add_subdirectory(cci-1.0.0)
        else()
         	add_subdirectory(cci-1.0.1)
        endif()
    endif()
    add_subdirectory(${lwtr4sc_SOURCE_DIR}/src "Gwtr4sc")
endif()
