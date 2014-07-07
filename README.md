# NETCONF frontend for mand

freenetconfd is NETCONF over ssh frontend for mand. It accepts incomming NETCONF
sessions and translates NETCONF requests and RPC to mand's dmconfig API.

# Building and Install

## Requirements

- GNU make
- cmake
- gcc >= 4.7
- libev (including the event.h compatibility header, libev-libevent-dev package on Debian/Ubuntu)
- libtalloc
- libssh
- libubox
- libuci
- libubus
- libroxml

## Build and Install

* generate makefiles

	cmake -DCMAKE_INSTALL_PREFIX=/usr

* build and install

	make
	make install