cmake_minimum_required(VERSION 2.8)

include(LitecashWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB litecash)

include_directories(${CMAKE_BINARY_DIR}
    src
    litecash/external
    litecash/include
    litecash/src)

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
configure_file("litecash/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  litecash/external/miniupnpc/connecthostport.c
  litecash/external/miniupnpc/igd_desc_parse.c
  litecash/external/miniupnpc/minisoap.c
  litecash/external/miniupnpc/miniupnpc.c
  litecash/external/miniupnpc/miniwget.c
  litecash/external/miniupnpc/minixml.c
  litecash/external/miniupnpc/portlistingparse.c
  litecash/external/miniupnpc/receivedata.c
  litecash/external/miniupnpc/upnpcommands.c
  litecash/external/miniupnpc/upnpreplyparse.c
  litecash/src/Common/Base58.cpp
  litecash/src/Common/CommandLine.cpp
  litecash/src/Common/Util.cpp
  litecash/src/Common/StringTools.cpp
  litecash/src/Common/JsonValue.cpp
  litecash/src/Common/ConsoleTools.cpp
  litecash/src/Common/MemoryInputStream.cpp
  litecash/src/Common/PathTools.cpp
  litecash/src/Common/StdInputStream.cpp
  litecash/src/Common/StdOutputStream.cpp
  litecash/src/Common/StreamTools.cpp
  litecash/src/Common/StringOutputStream.cpp
  litecash/src/Common/StringView.cpp
  litecash/src/Common/VectorOutputStream.cpp
  litecash/src/crypto/blake256.c
  litecash/src/crypto/chacha8.c
  litecash/src/crypto/crypto-ops-data.c
  litecash/src/crypto/crypto-ops.c
  litecash/src/crypto/crypto.cpp
  litecash/src/crypto/groestl.c
  litecash/src/crypto/hash-extra-blake.c
  litecash/src/crypto/hash-extra-groestl.c
  litecash/src/crypto/hash-extra-jh.c
  litecash/src/crypto/hash-extra-skein.c
  litecash/src/crypto/hash.c
  litecash/src/crypto/jh.c
  litecash/src/crypto/keccak.c
  litecash/src/crypto/oaes_lib.c
  litecash/src/crypto/random.c
  litecash/src/crypto/skein.c
  litecash/src/crypto/slow-hash.c
  litecash/src/crypto/slow-hash.cpp
  litecash/src/crypto/tree-hash.c
  litecash/src/CryptoNoteCore/BlockchainIndices.cpp
  litecash/src/CryptoNoteCore/BlockchainMessages.cpp
  litecash/src/CryptoNoteCore/BlockIndex.cpp
  litecash/src/CryptoNoteCore/CoreConfig.cpp
  litecash/src/CryptoNoteCore/CryptoNoteBasic.cpp
  litecash/src/CryptoNoteCore/CryptoNoteTools.cpp
  litecash/src/CryptoNoteCore/Currency.cpp
  litecash/src/CryptoNoteCore/MinerConfig.cpp
  litecash/src/CryptoNoteCore/Transaction.cpp
  litecash/src/CryptoNoteCore/Account.cpp
  litecash/src/CryptoNoteCore/Blockchain.cpp
  litecash/src/CryptoNoteCore/Checkpoints.cpp
  litecash/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  litecash/src/CryptoNoteCore/Core.cpp
  litecash/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  litecash/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  litecash/src/CryptoNoteCore/Difficulty.cpp
  litecash/src/CryptoNoteCore/IBlock.cpp
  litecash/src/CryptoNoteCore/Miner.cpp
  litecash/src/CryptoNoteCore/TransactionExtra.cpp
  litecash/src/CryptoNoteCore/TransactionPool.cpp
  litecash/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  litecash/src/CryptoNoteCore/TransactionUtils.cpp
  litecash/src/InProcessNode/InProcessNode.cpp
  litecash/src/InProcessNode/InProcessNodeErrors.cpp
  litecash/src/NodeRpcProxy/NodeErrors.cpp
  litecash/src/NodeRpcProxy/NodeRpcProxy.cpp
  litecash/src/P2p/NetNodeConfig.cpp
  litecash/src/Serialization/BinaryInputStreamSerializer.cpp
  litecash/src/Serialization/BinaryOutputStreamSerializer.cpp
  litecash/src/Serialization/JsonInputValueSerializer.cpp
  litecash/src/Serialization/JsonOutputStreamSerializer.cpp
  litecash/src/Serialization/KVBinaryInputStreamSerializer.cpp
  litecash/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  litecash/src/Serialization/SerializationOverloads.cpp
  litecash/src/Transfers/BlockchainSynchronizer.cpp
  litecash/src/Transfers/SynchronizationState.cpp
  litecash/src/Transfers/TransfersConsumer.cpp
  litecash/src/Transfers/TransfersContainer.cpp
  litecash/src/Transfers/TransfersSubscription.cpp
  litecash/src/Transfers/TransfersSynchronizer.cpp
  litecash/src/Wallet/LegacyKeysImporter.cpp
  litecash/src/Wallet/WalletAsyncContextCounter.cpp
  litecash/src/Wallet/WalletErrors.cpp
  litecash/src/WalletLegacy/KeysStorage.cpp
  litecash/src/WalletLegacy/WalletLegacy.cpp
  litecash/src/WalletLegacy/WalletHelper.cpp
  litecash/src/WalletLegacy/WalletLegacySerializer.cpp
  litecash/src/WalletLegacy/WalletLegacySerialization.cpp
  litecash/src/WalletLegacy/WalletTransactionSender.cpp
  litecash/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  litecash/src/WalletLegacy/WalletUserTransactionsCache.cpp
  litecash/src/System/ContextGroup.cpp
  litecash/src/System/Event.cpp
  litecash/src/System/EventLock.cpp
  litecash/src/System/InterruptedException.cpp
  litecash/src/System/Ipv4Address.cpp
  litecash/src/System/TcpStream.cpp
  litecash/src/HTTP/HttpRequest.cpp
  litecash/src/HTTP/HttpParser.cpp
  litecash/src/HTTP/HttpParserErrorCodes.cpp
  litecash/src/HTTP/HttpResponse.cpp
  litecash/src/Rpc/HttpClient.cpp
  litecash/src/Rpc/JsonRpc.cpp
  litecash/src/P2p/NetNode.cpp
  litecash/src/P2p/LevinProtocol.cpp
  litecash/src/P2p/NetNodeConfig.cpp
  litecash/src/P2p/PeerListManager.cpp
  litecash/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  litecash/src/Logging/ILogger.cpp
  litecash/src/Logging/LoggerGroup.cpp
  litecash/src/Logging/CommonLogger.cpp
  litecash/src/Logging/LoggerManager.cpp
  litecash/src/Logging/FileLogger.cpp
  litecash/src/Logging/StreamLogger.cpp
  litecash/src/Logging/ConsoleLogger.cpp
  litecash/src/Logging/LoggerMessage.cpp
  litecash/src/Logging/LoggerRef.cpp
  litecash/src/BlockchainExplorer/BlockchainExplorer.cpp
  litecash/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  litecash/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
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
  include_directories(litecash/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/litecashwallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} litecash/external/miniupnpc/minissdpc.c)
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

    set(MACOSX_BUNDLE_ICON_FILE litecash.icns)
    set(APPLICATION_ICON src/images/litecash.icns)
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

include_directories(litecash/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES litecash/src/Platform/${PLATFORM_DIR}/System/*)
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
