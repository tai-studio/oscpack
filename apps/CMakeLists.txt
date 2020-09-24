ADD_EXECUTABLE(OscDump OscDump.cpp)
TARGET_LINK_LIBRARIES(OscDump oscpack ${LIBS})

ADD_EXECUTABLE(SimpleReceive SimpleReceive.cpp)
TARGET_LINK_LIBRARIES(SimpleReceive oscpack ${LIBS})

ADD_EXECUTABLE(SimpleSend SimpleSend.cpp)
TARGET_LINK_LIBRARIES(SimpleSend oscpack ${LIBS})

