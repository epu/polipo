cmake_minimum_required(VERSION 3.2)

project (polipo)

if (MSVC)
	set(LOCAL_ROOT "C:/ProgramData/polipo/www" CACHE FILEPATH "Default hard-coded path for serving web files.")
	set(DISK_CACHE_ROOT "C:/ProgramData/polipo/cache" CACHE FILEPATH "Default hard-coded path for cache data.")
	set(POLIPO_GLOBAL_CONFIG "C:/ProgramData/polipo/config" CACHE FILEPATH "Default hard-coded path for config file.")
endif ()

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
		fts_compat.c
		socks.c
		mingw.c
)

# CMAKE_SYSTEM_NAME MATCHES "Windows"
if (MSVC)
	add_definitions("-DWIN32")
	if (CMAKE_GENERATOR MATCHES "Visual Studio 12")
		# we could set HAS_STDINT_H instead of having modified the md5.h
		add_definitions("-DHAS_STDINT_H" "-D_CRT_SECURE_NO_WARNINGS")
		add_definitions(-DLOCAL_ROOT="${LOCAL_ROOT}" -DDISK_CACHE_ROOT="${DISK_CACHE_ROOT}")
		add_definitions(-DPOLIPO_GLOBAL_CONFIG="${POLIPO_GLOBAL_CONFIG}")

		list(APPEND SRCS dirent_compat.c fts_compat.c msvc/getopt.c)
		list(REMOVE_ITEM SRCS md5import.c)
	endif()
endif ()

add_executable(polipo ${SRCS})

if (MSVC)
    # TODO: fix IDE projects to use the debug variant. -epu
	target_link_libraries(polipo ws2_32 libcpmt msvcmrt)
endif ()

install(TARGETS polipo
   RUNTIME
   DESTINATION bin)
