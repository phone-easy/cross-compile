NAME = ethumb
VERSION = 0.1
DEPENDS = edje e_dbus

SVN_URL = http://svn.enlightenment.org/svn/e/trunk/${NAME}

.PHONY: unpack
unpack: build/.unpacked

build/.unpacked:
	mkdir -p build/src
	svn checkout -q --non-interactive ${SVN_URL} build/src
	touch $@

update: build/.unpacked
	( cd build/src ; \
		if [ -n "`svn status | egrep -v '^\?'`" ]; then \
			svn update --non-interactive; \
			rm -f ../*.built; \
		fi )

include ../../support/staging.mk

include ../../support/cross-compile.mk

stage-local:: build/${ARCH}.built

build/%.built: build/.unpacked
	rm -f $@
	( cd build/src ; ${SB2} CFLAGS="-I${INSTALL_PREFIX}/include\ -g\ -O2" \
		LDFLAGS="-L${INSTALL_PREFIX}/lib\ -Wl,-rpath,${INSTALL_PREFIX}/lib" \
		PKG_CONFIG_PATH=${INSTALL_PREFIX}/lib/pkgconfig \
		./autogen.sh --disable-static --prefix=${INSTALL_PREFIX} --disable-doc )
	( cd build/src ; ${SB2} cp `which libtool` . )
	( cd build/src ; ${SB2} $(MAKE) && ${SB2} make install )
	touch $@

clobber::
	rm -rf build
