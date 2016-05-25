#
# Makefile version = "$Id: Makefile.in,v 1.5 2016/02/26 05:33:29 pberger Exp $"
#
prefix = /usr
datarootdir = ${prefix}/share

include ../Makefile.inc
#include the kernel headers if we had found em.
-include ../Makefile.kheaders

#
# linux commands
#

# SHELL=		/bin/bash
CC= gcc
CFLAGS = -O2
OPT=		-O

INC =

LIBS = -lncurses 
MANDIR=${datarootdir}/man

#
# Ditty compiler options
#

VERSION=  -DDIGI_VERSION=\"$(TRUE_VERSION)\"

APP_OPTS= $(VERSION)

#
# Production targets
#

all:	ditty

ditty:	ditty.c 
	$(CC) ${APP_OPTS} ${CFLAGS} -o ditty $(OPT) -D_DGRP_ -I../commoninc $(INC) ditty.c ${LIBS}
	: strip ditty

#
# Utility
#

install:
	install -d -m 755 $(DITTYBIN)
	install -d -m 755 $(RPM_BUILD_ROOT)$(MANDIR)/man1
	install -m 755 ditty $(DITTYBIN)/$(DITTYNAME)
	install -m 644 ditty.man $(RPM_BUILD_ROOT)$(MANDIR)/man1/$(DITTYNAME).1

# TODO: check and make sure no other packages are 
# using the ditty program
uninstall:
	-rm -f $(DITTYBIN)/$(DITTYNAME)
	-rm -f $(RPM_BUILD_ROOT)$(MANDIR)/man1/$(DITTYNAME).1*

clean: 
	rm -f *.o ditty

clobber:	clean
	rm -f *.o.??.?? ditty.??.??

.PHONY: all kernel clean clobber install uninstall
