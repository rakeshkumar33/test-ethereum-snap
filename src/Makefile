# Pssibled parameters:
#
# WITH_DBUS [ 1 | 0 ]
# ARCH      [ x86 | armhf ]
#    -x86 is basically the local machine
#    -armhf will only work for cross compiling to arm with the ubuntu cross compile toolchain
#
OUT_NAME=test-c-app

ARCH=x86
CC=gcc
INCLUDES=
LIBRARIES=
CCFLAGS=-Wall -Wextra
GOT_ARM_COMPILER:=$(shell hash arm-linux-gnueabihf-gcc 2>/dev/null; echo $$?)

# Set the compiler
ifeq ($(ARCH),armhf)
	ifeq ($(GOT_ARM_COMPILER),0)
		CC=arm-linux-gnueabihf-gcc
	else
		$(error Asked to build for arm without having a cross compiler)
	endif
endif

# Set our includes
ifeq ($(WITH_DBUS),1)
	ifeq ($(ARCH),x86)
		INCLUDES+=$(shell pkg-config dbus-1 --cflags)
	else ifeq ($(ARCH),armhf)
		INCLUDES+=-I/usr/include/dbus-1.0  -I/usr/lib/arm-linux-gnueabihf/dbus-1.0/include
	else
		$(error Unrecognized value for ARCH variable)
	endif
endif

# Set Libraries
ifeq ($(WITH_DBUS),1)
	LIBRARIES+=-ldbus-1
endif

SOURCES=main.c
ifeq ($(WITH_DBUS),1)
	SOURCES+=dbus.c
endif

testapp: $(SOURCES)
	$(CC) $(SOURCES) -o $(OUT_NAME)  $(INCLUDES) $(CCFLAGS) $(LIBRARIES)


.PHONY: clean

clean:
	rm -f $(OUT_NAME)
