cmake_minimum_required (VERSION 3.0.2)

project (strophe)

add_definitions( -DXML_STATIC )
add_subdirectory(expat)

#if (NOT ${OPENSSL_INCLUDES} OR NOT ${OPENSSL_LIBRARIES})
#	message (FATAL "-- OpenSSL not found")
#endif()

set (STROPHE_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_SOURCE_DIR}/include)
set (STROPHE_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)

include_directories (${STROPHE_INCLUDE_DIR})
include_directories (expat/lib)
include_directories (${OPENSSL_INCLUDE_DIR})
include_directories (${OPENSSL_INCLUDES})

set(STROPHE_SOURCE
		${STROPHE_SOURCE_DIR}/auth.c
		${STROPHE_SOURCE_DIR}/conn.c
		${STROPHE_SOURCE_DIR}/crypto.c
		${STROPHE_SOURCE_DIR}/ctx.c
		${STROPHE_SOURCE_DIR}/event.c
		${STROPHE_SOURCE_DIR}/handler.c
		${STROPHE_SOURCE_DIR}/hash.c
		${STROPHE_SOURCE_DIR}/jid.c
		${STROPHE_SOURCE_DIR}/parser_expat.c
		${STROPHE_SOURCE_DIR}/md5.c
		${STROPHE_SOURCE_DIR}/rand.c
		${STROPHE_SOURCE_DIR}/resolver.c
		${STROPHE_SOURCE_DIR}/sasl.c
		${STROPHE_SOURCE_DIR}/scram.c
		${STROPHE_SOURCE_DIR}/sha1.c
		${STROPHE_SOURCE_DIR}/snprintf.c
		${STROPHE_SOURCE_DIR}/sock.c
		${STROPHE_SOURCE_DIR}/stanza.c
		${STROPHE_SOURCE_DIR}/thread.c
		${STROPHE_SOURCE_DIR}/tls_openssl.c
		${STROPHE_SOURCE_DIR}/util.c
)

add_library(strophe STATIC ${STROPHE_SOURCE} )
add_dependencies (strophe expat)

# FIXME MSZG set target dependencies in a general way
if (WIN32)
	target_link_libraries (strophe ${LIBRARY_OUTPUT_PATH}/expat.lib ${OPENSSL_LIBRARIES})
else()
	target_link_libraries (strophe ${LIBRARY_OUTPUT_PATH}/libexpat.a ${OPENSSL_LIBRARIES})
endif()
