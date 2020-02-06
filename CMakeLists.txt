
cmake_minimum_required(VERSION 3.4.1)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED YES)

project(TEST)
enable_testing()

#Find openSSL
find_package(OpenSSL REQUIRED)

#Prep ourselves for compiling boost
find_package(Boost COMPONENTS 
    unit_test_framework REQUIRED
    atomic REQUIRED
    chrono REQUIRED
    context REQUIRED
    date_time REQUIRED
    exception REQUIRED
    filesystem REQUIRED
    graph REQUIRED
    iostreams REQUIRED
    math_c99 REQUIRED
    math_c99f REQUIRED
    math_c99l REQUIRED
    math_tr1 REQUIRED
    math_tr1f REQUIRED
    math_tr1l REQUIRED
    prg_exec_monitor REQUIRED
    program_options REQUIRED
    random REQUIRED
    regex REQUIRED
    serialization REQUIRED
    system REQUIRED
    test_exec_monitor REQUIRED
    thread REQUIRED
    timer REQUIRED
    wave REQUIRED
    wserialization REQUIRED
)

include_directories(${Boost_INCLUDE_DIRS})
include_directories("src")
include_directories("test")
include_directories("src/contrib")

set(MYCOINEVO_CORE_CPP "src/mycoinevo-core-cpp")
set(MYCOINEVO_CORE_CPP_SRC "${MYCOINEVO_CORE_CPP}/src")
include_directories("${MYCOINEVO_CORE_CPP}/src")
#
set(COINEVO_SRC "${MYCOINEVO_CORE_CPP}/contrib/coinevo-core-custom")
include_directories(${COINEVO_SRC})
include_directories("${COINEVO_SRC}/epee/include")
include_directories("${COINEVO_SRC}/common")
include_directories("${COINEVO_SRC}/vtlogger")
include_directories("${COINEVO_SRC}/crypto")
include_directories("${COINEVO_SRC}/cryptonote_basic")
include_directories("${COINEVO_SRC}/multisig")
include_directories("${COINEVO_SRC}/cryptonote_core")
include_directories("${COINEVO_SRC}/cryptonote_protocol")
include_directories("${COINEVO_SRC}/wallet")
include_directories("${COINEVO_SRC}/rpc")
include_directories("${COINEVO_SRC}/mnemonics")
include_directories("${COINEVO_SRC}/contrib/libsodium/include") # support sodium/… paths
include_directories("${COINEVO_SRC}/contrib/libsodium/include/sodium")


# keeping test files in a separate source directory 
file(GLOB TEST_SRCS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} test/test_*.cpp)

set(
    SRC_FILES
    #
    test/tests_common.hpp
    #
	src/Dispatch/Dispatch_Interface.hpp
	src/Dispatch/Dispatch.asio.hpp
	src/UserIdle/UserIdle.hpp
	src/UserIdle/UserIdle.cpp
    src/Passwords/PasswordController.hpp
    src/Passwords/PasswordController.cpp
	src/Settings/SettingsProviders.hpp
	src/Settings/SettingsProviders.cpp
    src/Settings/SettingsController.hpp
    src/Settings/SettingsController.cpp
	src/Lists/PersistedObjectListController.hpp
	src/Lists/PersistedObjectListController.cpp
	src/Wallets/Wallet.hpp
	src/Wallets/Wallet.cpp
	src/Wallets/WalletsListController_Base.hpp
	src/Wallets/WalletsListController_Base.cpp
	src/Wallets/WalletsListController.Full.hpp
	src/Wallets/WalletsListController.Full.cpp
	src/Wallets/WalletsListController.Lite.hpp
	src/Wallets/WalletsListController.Lite.cpp
	src/Wallets/Wallet_HostPollingController.hpp
	src/Wallets/Wallet_HostPollingController.cpp
	src/Wallets/Wallet_TxCleanupController.hpp
	src/Wallets/Wallet_TxCleanupController.cpp
	src/Wallets/Wallet_KeyImageCache.hpp
	src/Wallets/Wallet_KeyImageCache.cpp
    src/APIClient/root_certificates.hpp
    src/Currencies/Currencies.hpp
    src/Currencies/Currencies.cpp
    src/Persistence/document_persister.hpp
    src/Persistence/document_persister.cpp
    src/Persistence/PersistableObject.hpp
    src/Persistence/PersistableObject.cpp
    src/AppBridge/AppBridge.hpp
    src/AppBridge/AppBridge.cpp
    src/AppBridge/HTTPRequests_Handle_bridge.hpp
    src/App/AppServiceLocator.hpp
    src/App/AppServiceLocator.asio.cpp
    src/SendFunds/Controllers/SendFundsFormSubmissionController.hpp
    src/SendFunds/Controllers/SendFundsFormSubmissionController.cpp
    src/APIClient/HTTPRequests_Interface.hpp
    src/APIClient/HTTPRequests.beast.hpp
    src/APIClient/HostedCoinevo.cpp
    src/APIClient/HostedCoinevo.hpp
	src/APIClient/parsing.hpp
	src/OpenAlias/OpenAlias.hpp
	src/OpenAlias/OpenAlias.cpp
    src/RNCryptor-C/rncryptor_c.h
    src/RNCryptor-C/rncryptor_c.c
    src/RNCryptor-C/mutils.h
    src/RNCryptor-C/mutils.c
    src/base64/base64.hpp
    #
    src/contrib/rapidjson_defines.hpp
	#
	${MYCOINEVO_CORE_CPP_SRC}/coinevo_address_utils.hpp
	${MYCOINEVO_CORE_CPP_SRC}/coinevo_address_utils.cpp
	${MYCOINEVO_CORE_CPP_SRC}/coinevo_paymentID_utils.hpp
	${MYCOINEVO_CORE_CPP_SRC}/coinevo_paymentID_utils.cpp
	${MYCOINEVO_CORE_CPP_SRC}/coinevo_key_image_utils.hpp
	${MYCOINEVO_CORE_CPP_SRC}/coinevo_key_image_utils.cpp
	${MYCOINEVO_CORE_CPP_SRC}/coinevo_fee_utils.hpp
	${MYCOINEVO_CORE_CPP_SRC}/coinevo_fee_utils.cpp
	${MYCOINEVO_CORE_CPP_SRC}/coinevo_transfer_utils.hpp
	${MYCOINEVO_CORE_CPP_SRC}/coinevo_transfer_utils.cpp
	${MYCOINEVO_CORE_CPP_SRC}/coinevo_fork_rules.hpp
	${MYCOINEVO_CORE_CPP_SRC}/coinevo_fork_rules.cpp
	${MYCOINEVO_CORE_CPP_SRC}/coinevo_wallet_utils.hpp
	${MYCOINEVO_CORE_CPP_SRC}/coinevo_wallet_utils.cpp
	${MYCOINEVO_CORE_CPP_SRC}/serial_bridge_index.hpp
	${MYCOINEVO_CORE_CPP_SRC}/serial_bridge_index.cpp
	${MYCOINEVO_CORE_CPP_SRC}/coinevo_send_routine.hpp
	${MYCOINEVO_CORE_CPP_SRC}/coinevo_send_routine.cpp
	${MYCOINEVO_CORE_CPP_SRC}/serial_bridge_utils.hpp
	${MYCOINEVO_CORE_CPP_SRC}/serial_bridge_utils.cpp
	${MYCOINEVO_CORE_CPP_SRC}/tools__ret_vals.hpp
	${MYCOINEVO_CORE_CPP_SRC}/tools__ret_vals.cpp
	#
	${COINEVO_SRC}/cryptonote_basic/cryptonote_basic_impl.cpp
	${COINEVO_SRC}/cryptonote_basic/account.cpp
	${COINEVO_SRC}/cryptonote_basic/cryptonote_format_utils.cpp
	${COINEVO_SRC}/crypto/crypto.cpp
	${COINEVO_SRC}/crypto/hash.c
	${COINEVO_SRC}/crypto/slow-hash-dummied.cpp
	${COINEVO_SRC}/crypto/oaes_lib.c
	${COINEVO_SRC}/crypto/crypto-ops.c
	${COINEVO_SRC}/crypto/crypto-ops-data.c
	${COINEVO_SRC}/crypto/keccak.c
	${COINEVO_SRC}/crypto/chacha.c
	${COINEVO_SRC}/crypto/random.c
	${COINEVO_SRC}/crypto/aesb.c
	${COINEVO_SRC}/crypto/tree-hash.c
	${COINEVO_SRC}/crypto/hash-extra-blake.c
	${COINEVO_SRC}/crypto/blake256.c
	${COINEVO_SRC}/crypto/hash-extra-groestl.c
	${COINEVO_SRC}/crypto/hash-extra-jh.c
	${COINEVO_SRC}/crypto/hash-extra-skein.c
	${COINEVO_SRC}/crypto/groestl.c
	${COINEVO_SRC}/crypto/jh.c
	${COINEVO_SRC}/crypto/skein.c
	${COINEVO_SRC}/cryptonote_core/cryptonote_tx_utils.cpp
	${COINEVO_SRC}/common/base58.cpp
	${COINEVO_SRC}/common/threadpool.cpp
	${COINEVO_SRC}/common/aligned.c
	${COINEVO_SRC}/common/util.cpp
	${COINEVO_SRC}/epee/src/hex.cpp
	${COINEVO_SRC}/epee/src/string_tools.cpp
	${COINEVO_SRC}/epee/src/memwipe.c
	${COINEVO_SRC}/epee/src/mlocker.cpp
	${COINEVO_SRC}/epee/src/wipeable_string.cpp
	${COINEVO_SRC}/device/device.cpp
	${COINEVO_SRC}/device/device_default.cpp
	${COINEVO_SRC}/ringct/rctOps.cpp
	${COINEVO_SRC}/ringct/rctTypes.cpp
	${COINEVO_SRC}/ringct/rctCryptoOps.c
	${COINEVO_SRC}/ringct/rctSigs.cpp
	${COINEVO_SRC}/ringct/bulletproofs.cc
	${COINEVO_SRC}/ringct/multiexp.cc
	${COINEVO_SRC}/mnemonics/electrum-words.cpp
	${COINEVO_SRC}/vtlogger/logger.cpp
	${COINEVO_SRC}/contrib/libsodium/src/crypto_verify/verify.c
)
# needed for slow-hash
add_compile_options(-maes)

#Run through each source
foreach(testSrc ${TEST_SRCS})
    # extract the filename without an extension (NAME_WE)
    get_filename_component(testName ${testSrc} NAME_WE)
    add_executable(
        ${testName}
        ${testSrc}
        ${SRC_FILES}
    )
    target_link_libraries(
        ${testName}
        #
        OpenSSL::SSL
        ${Boost_LIBRARIES} 
    )
    set_target_properties(
        ${testName} PROPERTIES 
        RUNTIME_OUTPUT_DIRECTORY
        ${CMAKE_CURRENT_SOURCE_DIR}/build/products
    )
    add_test(
        NAME ${testName} 
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/build/products 
        COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/build/products/${testName} --catch_system_error=yes
    )
endforeach(testSrc)
