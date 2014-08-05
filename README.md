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

# Adding new YANG modules

freenetconfd maps NETCONF requests to mand's API, but it is *NOT* aware of the underlying
YANG models. This leads to some restrictions:
* the mandatory node attribute is not enforced
* the read-only node attribute is not enforced
* manual implementated support code is needed for:
  * special namespace prefixes
  * lists
  * leaf lists
  * leafrefs
  * RPC's

mand does not preserve the namespace prefixes of the original YANG model. However, NETCONF
expects some nodes to contain those prefixes. Freenetconfd need support code in src/dmconfig.c,
dm_get_xml_config to add those prefixes.

freenetconfd needs to know which nodes are leafrefs, list and leaf list to properly translate them.
The definition list is in the head of src/dmconfig.c

```
const char *list_keys[] = {
	"ip",
	"name"
};

const char *leafrefs[] = {
	"lower-layer-if",
	"higher-layer-if"
};

const char *leaf_list_nodes[] = {
	"higher-layer-if", "lower-layer-if",
	"user-name", "search",
	"user-authentication-order", "bootorder"
};
```

Just like in mand, NETCONF RPC to libdmconfig RPC mappings need manual implementation. For
exisiting implementations check the `dm_rpc_` family of functions in src/dmconfig.c.