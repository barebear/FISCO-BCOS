
set(SOURCES
	ExtVMFace.cpp
	VM.cpp
	VMOpt.cpp
	VMCalls.cpp
	VMFactory.cpp
	CoverTool.cpp
	./ethcall/EthCallEntry.cpp
	./ethcall/EthLog.cpp
	./ethcall/Paillier.cpp
    ./ethcall/TrieProof.cpp
    ./ethcall/VerifySign.cpp
)

if (EVMJIT)
	list(APPEND SOURCES
		JitVM.cpp
		SmartVM.cpp
	)
endif()

file(GLOB HEADERS "*.h" " ./ethcall/*.h")

add_library(evm ${SOURCES} ${HEADERS})

find_package(Eth)

target_link_libraries(evm ${Eth_ETHCORE_LIBRARIES})
target_link_libraries(evm ${Eth_EVMCORE_LIBRARIES})
target_link_libraries(evm paillier)
target_include_directories(evm PRIVATE .. ${JSONCPP_INCLUDE_DIR})

if (EVMJIT)
	target_include_directories(evm PRIVATE ../evmjit/include)
	target_link_libraries(evm ${Eth_EVMJIT_LIBRARIES})
endif()

install(TARGETS evm RUNTIME DESTINATION bin ARCHIVE DESTINATION lib LIBRARY DESTINATION lib)
