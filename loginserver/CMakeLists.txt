CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

SET(eqlogin_sources
    Client.cpp
    ClientManager.cpp
    Config.cpp
    DatabaseMySQL.cpp
    DatabasePostgreSQL.cpp
    Encryption.cpp
    ErrorLog.cpp
    Main.cpp
    ServerManager.cpp
    WorldServer.cpp
)

SET(eqlogin_headers
    Client.h
    ClientManager.h
    Config.h
    Database.h
    DatabaseMySQL.h
    DatabasePostgreSQL.h
    Encryption.h
    EQCryptoAPI.h
    ErrorLog.h
    LoginServer.h
    LoginStructures.h
    Options.h
    ServerManager.h
    WorldServer.h
)

ADD_EXECUTABLE(loginserver ${eqlogin_sources} ${eqlogin_headers})

TARGET_LINK_LIBRARIES(loginserver Common debug ${MySQL_LIBRARY_DEBUG} optimized ${MySQL_LIBRARY_RELEASE})

IF(MSVC)
	
    SET_TARGET_PROPERTIES(loginserver PROPERTIES LINK_FLAGS_RELEASE "/OPT:REF /OPT:ICF")
    TARGET_LINK_LIBRARIES(loginserver "Ws2_32.lib")
ENDIF(MSVC)

IF(MINGW)
    TARGET_LINK_LIBRARIES(loginserver "WS2_32")
ENDIF(MINGW)

IF(UNIX)
    TARGET_LINK_LIBRARIES(loginserver "dl")
    TARGET_LINK_LIBRARIES(loginserver "z")
    TARGET_LINK_LIBRARIES(loginserver "m")
    TARGET_LINK_LIBRARIES(loginserver "rt")
    TARGET_LINK_LIBRARIES(loginserver "pthread")
    ADD_DEFINITIONS(-fPIC)
ENDIF(UNIX)

SET(EXECUTABLE_OUTPUT_PATH ../Bin)
