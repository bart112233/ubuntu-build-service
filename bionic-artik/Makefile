#!/bin/sh

# (C) 2012-2015 Fathi Boudra <fathi.boudra@linaro.org>

# Calls all necessary live-build programs in the correct order to complete
# the bootstrap, chroot, binary, and source stage.

# You need live-build package installed and superuser privileges.

BUILD_NUMBER?=1
BASEIMG=bionic-artik
IMAGEPREFIX?=$(BASEIMG)-`date +%Y%m%d`-$(BUILD_NUMBER)
LOGFILE=$(IMAGEPREFIX).build-log.txt
CONFIGFILE=$(IMAGEPREFIX).config.tar.bz2
LISTFILE=$(IMAGEPREFIX).contents
PKGSFILE=$(IMAGEPREFIX).packages
TARGZFILE=$(IMAGEPREFIX).tar.gz
MD5SUMSFILE=$(IMAGEPREFIX).md5sums.txt
SHA1SUMSFILE=$(IMAGEPREFIX).sha1sums.txt

all:
	set +e; sudo lb build --debug --verbose 2>&1 | tee $(LOGFILE)
	if [ -f binary-tar.tar.gz ]; then \
		mkdir tmp_rootfs; \
		cd tmp_rootfs; sudo tar xf ../binary-tar.tar.gz ; \
		sudo mv binary/* .; sudo rmdir binary; \
		sudo tar zcf ../binary-tar.tar.gz *; \
		cd ..; sudo rm -rf tmp_rootfs; \
		tar -jcf $(CONFIGFILE) auto/ config/ configure; \
		sudo mv binary.contents $(LISTFILE); \
		sudo mv chroot.packages.live $(PKGSFILE); \
		sudo mv binary-tar.tar.gz $(TARGZFILE); \
		md5sum $(LOGFILE) $(CONFIGFILE) $(LISTFILE) $(PKGSFILE) $(TARGZFILE) > $(MD5SUMSFILE); \
		sha1sum $(LOGFILE) $(CONFIGFILE) $(LISTFILE) $(PKGSFILE) $(TARGZFILE) > $(SHA1SUMSFILE); \
	fi

clean:
	sudo lb clean --purge
	rm -f $(BASEIMG)-*
	rm -rf config
