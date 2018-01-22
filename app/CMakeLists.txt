# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.4.1)

include_directories(src/main/include)
include_directories(src/main/cpp/Common)

file(GLOB_RECURSE BlockchainExplorer src/main/cpp/BlockchainExplorer/*)
file(GLOB_RECURSE Common src/main/cpp/Common/*)
file(GLOB_RECURSE ConnectivityTool src/main/cpp/ConnectivityTool/*)
file(GLOB_RECURSE Crypto src/main/cpp/crypto/*)
file(GLOB_RECURSE CryptoNoteCore src/main/cpp/CryptoNoteCore/*)
file(GLOB_RECURSE CryptoNoteProtocol src/main/cpp/CryptoNoteProtocol/*)
file(GLOB_RECURSE Daemon src/main/cpp/Daemon/*)
file(GLOB_RECURSE Http src/main/cpp/HTTP/*)
file(GLOB_RECURSE InProcessNode src/main/cpp/InProcessNode/*)
file(GLOB_RECURSE Logging src/main/cpp/Logging/*)
file(GLOB_RECURSE NodeRpcProxy src/main/cpp/NodeRpcProxy/*)
file(GLOB_RECURSE P2p src/main/cpp/P2p/*)
file(GLOB_RECURSE Rpc src/main/cpp/Rpc/*)
file(GLOB_RECURSE Serialization src/main/cpp/Serialization/*)
file(GLOB_RECURSE SimpleWallet src/main/cpp/SimpleWallet/*)
if(MSVC)
file(GLOB_RECURSE System System/* Platform/Windows/System/*)
elseif(APPLE)
file(GLOB_RECURSE System System/* Platform/OSX/System/* Platform/Posix/System/*)
else()
file(GLOB_RECURSE System System/* Platform/Linux/System/* Platform/Posix/System/*)
endif()
file(GLOB_RECURSE Transfers src/main/cpp/Transfers/*)
file(GLOB_RECURSE Wallet src/main/cpp/Wallet/*)
file(GLOB_RECURSE WalletLegacy src/main/cpp/WalletLegacy/*)

file(GLOB_RECURSE JsonRpcServer src/main/cpp/JsonRpcServer/*)

file(GLOB_RECURSE PaymentGate src/main/cpp/PaymentGate/*)
file(GLOB_RECURSE PaymentGateService src/main/cpp/PaymentGateService/*)
file(GLOB_RECURSE Miner src/main/cpp/Miner/*)

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

source_group("" FILES $${Common} ${ConnectivityTool} ${Crypto} ${CryptoNoteCore} ${CryptoNoteProtocol} ${Daemon} ${JsonRpcServer} ${Http} ${Logging} ${NodeRpcProxy} ${P2p} ${Rpc} ${Serialization} ${SimpleWallet} ${System} ${Transfers} ${Wallet} ${WalletLegacy})

add_library( # Sets the name of the library.
             native-lib

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             src/main/cpp/native-lib.cpp )

add_library(BlockchainExplorer SHARED ${BlockchainExplorer})
add_library(Common SHARED ${Common})
add_library(Crypto SHARED ${Crypto})
add_library(CryptoNoteCore SHARED ${CryptoNoteCore})
add_library(Http SHARED ${Http})
add_library(InProcessNode SHARED ${InProcessNode})
add_library(Logging SHARED ${Logging})
add_library(NodeRpcProxy SHARED ${NodeRpcProxy})
add_library(Rpc SHARED ${Rpc})
add_library(P2P SHARED ${CryptoNoteProtocol} ${P2p})
add_library(Serialization SHARED ${Serialization})
#add_library(System SHARED ${System})
add_library(Transfers SHARED ${Transfers})
add_library(Wallet SHARED ${Wallet} ${WalletLegacy})
add_library(PaymentGate SHARED ${PaymentGate})
add_library(JsonRpcServer SHARED ${JsonRpcServer})

# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

target_link_libraries( # Specifies the target library.
                       native-lib

                       # Links the target library to the log library
                       # included in the NDK.
                       ${log-lib} )
