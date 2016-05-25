PROJECT=robotjs
SOURCES=xdisplay.c deadbeef_rand.c mouse.c keypress.c keycode.c screen.c screengrab.c snprintf.c MMBitmap.c robotjs.cc
LIBRARY=nope
INCPATHS=/usr/include/X11 /usr/include/X11/extensions /usr/include/node
LIBPATHS=/usr/lib64/
LDFLAGS=-lmysqlclient -lmemcached -lmemcachedutil
CFLAGS=-c -Wall $(mysql_config --cflags)
CC=gcc
 
# ------------ MAGIC BEGINS HERE -------------
 
# Automatic generation of some important lists
OBJECTS=$(SOURCES:.c=.o) $(SOURCES:.cc=.o)
INCFLAGS=$(foreach TMP,$(INCPATHS),-I$(TMP)) $(libpng-config  --cflags)
LIBFLAGS=$(foreach TMP,$(LIBPATHS),-L$(TMP)) $(libpng-config  --libs) -lz -lX11 -lXtst

# Set up the output file names for the different output types
ifeq "$(LIBRARY)" "shared"
    BINARY=lib$(PROJECT).so
    LDFLAGS += -shared
else ifeq "$(LIBRARY)" "static"
    BINARY=lib$(PROJECT).a
else
    BINARY=$(PROJECT)
endif

all: $(SOURCES) $(BINARY)

$(BINARY): $(OBJECTS)
    # Link the object files, or archive into a static library
    ifeq "$(LIBRARY)" "static"
        ar rcs $(BINARY) $(OBJECTS)
    else
        $(CC) $(LIBFLAGS) $(OBJECTS) $(LDFLAGS) -o $@
    endif

.c.o:
        $(CC) $(INCFLAGS) $(CFLAGS) -O3 -fPIC $< -o $@

.cc.o:
        $(CC) $(INCFLAGS) $(CFLAGS) -O3 -fPIC $< -o $@

distclean: clean
        rm -f $(BINARY)

clean:
        rm -f $(OBJECTS)
