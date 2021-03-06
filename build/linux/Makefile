#
# %W% %E%
# Copyright 1999 Sun Microsystems, Inc.
#

DIRS += \
	com/sun/media/customizer

DIRS += \
	images \
	javax/media \
        com/sun/media \
	com/ibm/media \
	native \
	jmapps \
	registry

TOPDIR = ../..
PLATFORM = linux
include $(TOPDIR)/build/$(PLATFORM)/makefiles/Makefile.defs

IMAGEDIR = $(TOPDIR)/build/linux/image
TESTPLAYER = $(TOPDIR)/test/player/
TESTPLAYERFILE = $(TOPDIR)/test/player/file/
TESTREGISTRY = $(TOPDIR)/test/registry/
TESTPLUGIN = $(TOPDIR)/test/plugin/
TESTPLAYERRTP = $(TESTPLAYER)rtp/
SAMPLESDIR = $(IMAGEDIR)/samples
SAMPLESRTPDIR = $(SAMPLESDIR)/rtp

BEANDIR = javax/media/bean/playerbean
MANIFEST = $(TOPDIR)/src/share/$(BEANDIR)/manifest.txt

all:
	$(MAKE) SCSL $(DIRS) TARGET=install

#include $(TOPDIR)/build/minclude/test_samples.jmk

image: cleanimage createdirs
	$(ECHO) Making image
	$(ECHO) -- Deleting unnecessary files
	$(RM) $(DESTDIR)/javax/media/util/*DataSource*
	$(ECHO) -- Copying libraries
	$(CP) $(DESTDIR)/*.so $(IMAGEDIR)/lib
	$(CP) $(DESTDIR)/jmf.properties $(IMAGEDIR)/lib
	$(ECHO) -- Copying javasound stuff
	$(CP) $(SOUNDCLASSES)/linux/libjsound.so $(IMAGEDIR)/lib
	$(CP) $(SOUNDCLASSES)/sound.jar $(SOUNDCLASSES)/soundbank.gm $(IMAGEDIR)/lib
	$(ECHO) -- Creating jmf.jar file
	$(CMD_PREFIX)cd $(DESTDIR) ; $(JAVAHOME)/bin/jar cf ../$(IMAGEDIR)/lib/jmf.jar *.class codecLib javax com jmapps ; cd ..
	$(ECHO) -- Creating mediaplayer.jar file
	$(CP) $(TOPDIR)/src/share/$(BEANDIR)/*.gif $(DESTDIR)/$(BEANDIR)
	$(CMD_PREFIX)cd $(DESTDIR) ; $(JAVAHOME)/bin/jar cfm \
		../$(IMAGEDIR)/lib/mediaplayer.jar ../$(MANIFEST) $(BEANDIR) ; cd ..
	$(RM) $(DESTDIR)/$(BEANDIR)/*.gif
	$(CP) -r $(TOPDIR)/src/solaris/bin/* $(IMAGEDIR)/bin
	$(RM) -r $(IMAGEDIR)/bin/SCCS
	$(RM) $(IMAGEDIR)/bin/jmfcustomizer
	$(CMD_PREFIX)chmod 777 $(IMAGEDIR)/bin/*
	$(CP) -r $(TOPDIR)/doc/share/* $(IMAGEDIR)/doc
	$(RM) -r $(IMAGEDIR)/doc/SCCS
	$(ECHO) Done creating image

bundle:
	$(RM) -rf JMF2.1.1
	mv $(IMAGEDIR) JMF2.1.1
	$(JAVAHOME)/bin/jar cfM jmf-2_1_1-linux.zip JMF2.1.1

full: all image bundle

createdirs:
	$(MKDIR) $(IMAGEDIR)/doc $(IMAGEDIR)/bin $(IMAGEDIR)/lib

cleanimage:
	$(RM) -rf $(IMAGEDIR)


# install:	Generate installed hierarchy in DESTDIR
# packages:	Generate CD-ROM image in IMAGEDIR

TARGETS = install objects packages targets sources sccs_clean sbtags sblink sblink_clean

.KEEP_STATE:

$(TARGETS):
	$(MAKE) $(DIRS) TARGET=$@

$(DIRS):: FORCE
	cd $@; $(MAKE) $(TARGET)



javadoc:
	mkdir -p $(TOPDIR)/build/javadoc
	$(JAVAHOME)/bin/javadoc -d $(TOPDIR)/build/javadoc \
		-sourcepath $(TOPDIR)/src/share \
		javax.media \
		javax.media.protocol \
		javax.media.control \
		javax.media.format \
		javax.media.renderer \
		javax.media.datasink \
		javax.media.util \
		javax.media.rtp \
		javax.media.rtp.session \
		javax.media.rtp.event \
		javax.media.rtp.rtcp

clean:
	$(RM) -rf lib
	$(MAKE) $(DIRS) TARGET=clean

SCSL:
	$(ECHO) Copy class files and libs for which source is not included
	$(MKDIR) lib
	$(CP) -rp ../../derived/com lib
	$(CP) -rp ../../derived_linux/. lib	

FORCE:
