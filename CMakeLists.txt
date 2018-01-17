cmake_minimum_required(VERSION 2.8)

include(CryptoCoynWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB cryptonote)

include_directories(${CMAKE_BINARY_DIR}
    src
    cryptocoynwallet/external
    cryptocoynwallet/include
    cryptocoynwallet/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.55 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "")
configure_file("cryptocoynwallet/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  cryptocoynwallet/external/miniupnpc/connecthostport.c
  cryptocoynwallet/external/miniupnpc/igd_desc_parse.c
  cryptocoynwallet/external/miniupnpc/minisoap.c
  cryptocoynwallet/external/miniupnpc/miniupnpc.c
  cryptocoynwallet/external/miniupnpc/miniwget.c
  cryptocoynwallet/external/miniupnpc/minixml.c
  cryptocoynwallet/external/miniupnpc/portlistingparse.c
  cryptocoynwallet/external/miniupnpc/receivedata.c
  cryptocoynwallet/external/miniupnpc/upnpcommands.c
  cryptocoynwallet/external/miniupnpc/upnpreplyparse.c
  cryptocoynwallet/src/Common/Base58.cpp
  cryptocoynwallet/src/Common/CommandLine.cpp
  cryptocoynwallet/src/Common/Util.cpp
  cryptocoynwallet/src/Common/StringTools.cpp
  cryptocoynwallet/src/Common/JsonValue.cpp
  cryptocoynwallet/src/Common/ConsoleTools.cpp
  cryptocoynwallet/src/Common/MemoryInputStream.cpp
  cryptocoynwallet/src/Common/PathTools.cpp
  cryptocoynwallet/src/Common/StdInputStream.cpp
  cryptocoynwallet/src/Common/StdOutputStream.cpp
  cryptocoynwallet/src/Common/StreamTools.cpp
  cryptocoynwallet/src/Common/StringOutputStream.cpp
  cryptocoynwallet/src/Common/StringView.cpp
  cryptocoynwallet/src/Common/VectorOutputStream.cpp
  cryptocoynwallet/src/crypto/blake256.c
  cryptocoynwallet/src/crypto/chacha8.c
  cryptocoynwallet/src/crypto/crypto-ops-data.c
  cryptocoynwallet/src/crypto/crypto-ops.c
  cryptocoynwallet/src/crypto/crypto.cpp
  cryptocoynwallet/src/crypto/groestl.c
  cryptocoynwallet/src/crypto/hash-extra-blake.c
  cryptocoynwallet/src/crypto/hash-extra-groestl.c
  cryptocoynwallet/src/crypto/hash-extra-jh.c
  cryptocoynwallet/src/crypto/hash-extra-skein.c
  cryptocoynwallet/src/crypto/hash.c
  cryptocoynwallet/src/crypto/jh.c
  cryptocoynwallet/src/crypto/keccak.c
  cryptocoynwallet/src/crypto/oaes_lib.c
  cryptocoynwallet/src/crypto/random.c
  cryptocoynwallet/src/crypto/skein.c
  cryptocoynwallet/src/crypto/slow-hash.c
  cryptocoynwallet/src/crypto/slow-hash.cpp
  cryptocoynwallet/src/crypto/tree-hash.c
  cryptocoynwallet/src/CryptoNoteCore/BlockchainIndices.cpp
  cryptocoynwallet/src/CryptoNoteCore/BlockchainMessages.cpp
  cryptocoynwallet/src/CryptoNoteCore/BlockIndex.cpp
  cryptocoynwallet/src/CryptoNoteCore/CoreConfig.cpp
  cryptocoynwallet/src/CryptoNoteCore/CryptoNoteBasic.cpp
  cryptocoynwallet/src/CryptoNoteCore/CryptoNoteTools.cpp
  cryptocoynwallet/src/CryptoNoteCore/Currency.cpp
  cryptocoynwallet/src/CryptoNoteCore/MinerConfig.cpp
  cryptocoynwallet/src/CryptoNoteCore/Transaction.cpp
  cryptocoynwallet/src/CryptoNoteCore/Account.cpp
  cryptocoynwallet/src/CryptoNoteCore/Blockchain.cpp
  cryptocoynwallet/src/CryptoNoteCore/Checkpoints.cpp
  cryptocoynwallet/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  cryptocoynwallet/src/CryptoNoteCore/Core.cpp
  cryptocoynwallet/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  cryptocoynwallet/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  cryptocoynwallet/src/CryptoNoteCore/Difficulty.cpp
  cryptocoynwallet/src/CryptoNoteCore/IBlock.cpp
  cryptocoynwallet/src/CryptoNoteCore/Miner.cpp
  cryptocoynwallet/src/CryptoNoteCore/TransactionExtra.cpp
  cryptocoynwallet/src/CryptoNoteCore/TransactionPool.cpp
  cryptocoynwallet/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  cryptocoynwallet/src/CryptoNoteCore/TransactionUtils.cpp
  cryptocoynwallet/src/InProcessNode/InProcessNode.cpp
  cryptocoynwallet/src/InProcessNode/InProcessNodeErrors.cpp
  cryptocoynwallet/src/NodeRpcProxy/NodeErrors.cpp
  cryptocoynwallet/src/NodeRpcProxy/NodeRpcProxy.cpp
  cryptocoynwallet/src/P2p/NetNodeConfig.cpp
  cryptocoynwallet/src/Serialization/BinaryInputStreamSerializer.cpp
  cryptocoynwallet/src/Serialization/BinaryOutputStreamSerializer.cpp
  cryptocoynwallet/src/Serialization/JsonInputValueSerializer.cpp
  cryptocoynwallet/src/Serialization/JsonOutputStreamSerializer.cpp
  cryptocoynwallet/src/Serialization/KVBinaryInputStreamSerializer.cpp
  cryptocoynwallet/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  cryptocoynwallet/src/Serialization/SerializationOverloads.cpp
  cryptocoynwallet/src/Transfers/BlockchainSynchronizer.cpp
  cryptocoynwallet/src/Transfers/SynchronizationState.cpp
  cryptocoynwallet/src/Transfers/TransfersConsumer.cpp
  cryptocoynwallet/src/Transfers/TransfersContainer.cpp
  cryptocoynwallet/src/Transfers/TransfersSubscription.cpp
  cryptocoynwallet/src/Transfers/TransfersSynchronizer.cpp
  cryptocoynwallet/src/Wallet/LegacyKeysImporter.cpp
  cryptocoynwallet/src/Wallet/WalletAsyncContextCounter.cpp
  cryptocoynwallet/src/Wallet/WalletErrors.cpp
  cryptocoynwallet/src/WalletLegacy/KeysStorage.cpp
  cryptocoynwallet/src/WalletLegacy/WalletLegacy.cpp
  cryptocoynwallet/src/WalletLegacy/WalletHelper.cpp
  cryptocoynwallet/src/WalletLegacy/WalletLegacySerializer.cpp
  cryptocoynwallet/src/WalletLegacy/WalletLegacySerialization.cpp
  cryptocoynwallet/src/WalletLegacy/WalletTransactionSender.cpp
  cryptocoynwallet/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  cryptocoynwallet/src/WalletLegacy/WalletUserTransactionsCache.cpp
  cryptocoynwallet/src/System/ContextGroup.cpp
  cryptocoynwallet/src/System/Event.cpp
  cryptocoynwallet/src/System/EventLock.cpp
  cryptocoynwallet/src/System/InterruptedException.cpp
  cryptocoynwallet/src/System/Ipv4Address.cpp
  cryptocoynwallet/src/System/TcpStream.cpp
  cryptocoynwallet/src/HTTP/HttpRequest.cpp
  cryptocoynwallet/src/HTTP/HttpParser.cpp
  cryptocoynwallet/src/HTTP/HttpParserErrorCodes.cpp
  cryptocoynwallet/src/HTTP/HttpResponse.cpp
  cryptocoynwallet/src/Rpc/HttpClient.cpp
  cryptocoynwallet/src/Rpc/JsonRpc.cpp
  cryptocoynwallet/src/P2p/NetNode.cpp
  cryptocoynwallet/src/P2p/LevinProtocol.cpp
  cryptocoynwallet/src/P2p/NetNodeConfig.cpp
  cryptocoynwallet/src/P2p/PeerListManager.cpp
  cryptocoynwallet/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  cryptocoynwallet/src/Logging/ILogger.cpp
  cryptocoynwallet/src/Logging/LoggerGroup.cpp
  cryptocoynwallet/src/Logging/CommonLogger.cpp
  cryptocoynwallet/src/Logging/LoggerManager.cpp
  cryptocoynwallet/src/Logging/FileLogger.cpp
  cryptocoynwallet/src/Logging/StreamLogger.cpp
  cryptocoynwallet/src/Logging/ConsoleLogger.cpp
  cryptocoynwallet/src/Logging/LoggerMessage.cpp
  cryptocoynwallet/src/Logging/LoggerRef.cpp
  cryptocoynwallet/src/BlockchainExplorer/BlockchainExplorer.cpp
  cryptocoynwallet/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  cryptocoynwallet/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(cryptocoynwallet/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} cryptocoynwallet/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Cryptonote GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME CryptonoteWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE cryptonote.icns)
    set(APPLICATION_ICON src/images/cryptonote.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})
	
    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(cryptocoynwallet/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES cryptocoynwallet/src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
