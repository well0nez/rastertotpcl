# default install paths
EXEC        = rastertotpcl
CUPSDIR     = $(shell cups-config --serverbin)
CUPSDATADIR = $(shell cups-config --datadir)
PPDPATH=/usr/share/ppd

# MacOS system integrity protection prevents us from writing to $(cups-config --datadir)
# As a fallback, we copy the PPDs to $(cups-config --serverroot)/ppd
UNAME_S     = $(shell uname -s)
ifeq ($(UNAME_S),Darwin)
CUPSDATADIR = $(shell cups-config --serverroot)
endif

CFLAGS  += $(shell cups-config --cflags)
CFLAGS  += -lm				# link math.h
CFLAGS  += -Wall			# enable all compiler warning messages
CFLAGS  += -Wno-deprecated-declarations	-Wall -Wextra --std=c99 -O2 # do not warn about deprecated CUPS api

LDFLAGS += $(shell cups-config --ldflags)
LDLIBS  += $(shell cups-config --image --libs)

all: rastertotpcl ppd

.PHONY: all ppd install uninstall clean

rastertotpcl:
	gcc -Wall -Wextra --std=c99 -O2 -lm rastertotpcl.c -o rastertotpcl -lcupsimage -lcups

ppd:
	ppdc tectpcl2.drv

install:
	install -s $(EXEC) $(CUPSDIR)/filter/
	install -m 644 tectpcl2.drv $(CUPSDATADIR)/drv/
	install -m 644 labelmedia.h $(CUPSDATADIR)/ppdc/
	cp ppd/* $(PPDPATH)/



uninstall:
	rm -f $(CUPSDIR)/filter/$(EXEC)
ifeq ($(UNAME_S),Darwin)
	rm -rf $(CUPSDATADIR)/ppd/$(EXEC)
else
	rm -f $(CUPSDATADIR)/drv/tectpcl2.drv
	rm -f $(CUPSDATADIR)/ppdc/labelmedia.h
endif

clean:
	rm -f $(EXEC)
	rm -rf ppd
