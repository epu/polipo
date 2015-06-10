cmake_minimum_required(VERSION 3.2)

project (polipo)

set (SRCS
		util.c
		event.c
		io.c
		chunk.c
		atom.c
		object.c
		log.c
		diskcache.c
		main.c
		config.c
		local.c
		http.c
		client.c
		server.c
		auth.c
		tunnel.c
		http_parse.c
		parse_time.c
		dns.c
		forbidden.c
		md5import.c
		md5.c
		ftsimport.c
		fts_compat.c
		socks.c
		mingw.c
)

# CMAKE_SYSTEM_NAME MATCHES "Windows"
if (MSVC)
	add_definitions("-DWIN32")
endif ()

add_executable(polipo ${SRCS})