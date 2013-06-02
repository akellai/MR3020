# Copyright (C) 2006 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# $Id: Makefile 5624 2007-09-04 00:29:07Z Dottout $

include $(TOPDIR)/rules.mk
# we use the $LINUX_DIR and $TARGET_MODULES_DIR variables, 
# which are defined in kernel.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=lirc
PKG_VERSION:=0.9.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=@SF/${PKG_NAME}

include $(INCLUDE_DIR)/package.mk

define Package/lirc/Default
	SECTION:=utils
	CATEGORY:=Utilities
	URL:=http://lirc.org/
endef

define Package/lirc
	$(call Package/lirc/Default)
	TITLE:=LIRC - Linux Infrared Remote Control
	DEPENDS:=@!LINUX_2_4
	MENU:=1
endef

define Package/lirc/description
	LIRC is a package that allows you to decode and send 
	infra-red signals of many (but not all) commonly used 
	remote controls.
	This package contains only lircd and lircd.conf
	Make sure you select then correct driver!
endef

define Package/lirc/config
	config LIRC_DRIVER
		string
		prompt "Driver to be complined with LIRC Daemon" if PACKAGE_lirc
		default none
endef

define  Package/lircdaemonadd
	$(call Package/lirc/Default)
	DEPENDS:=lirc
	TITLE:=Daemon Additional Files
endef

define Package/lircdaemonadd/description
  Contains those additional daemon-tools:
   -irrecord
   -lircmd
endef

define Package/lirctools
	$(call Package/lirc/Default)
	DEPENDS:=lirc
	TITLE:=LIRC tools
endef

define Package/lirctools/description
  This package contains the rest of LIRC daemons and tools:
   -irrecord
   -lircmd
   -ircat:   prints config strings to standard output, can be used to provide remote control input to scripts
   -irexec:  execute programs according to the pressed remote control buttons
   -irpty:   pseudo tty driver. Connects to lircd via socket to receive infra-red codes and converts them to key strokes
   -irsend:  application for sending IR-codes via lirc
   -irw:     watch the codes as lircd recognize them
   -lircrcd: daemon that manages current mode for all applications
   -mode2:   shows the pulse/space length of a remote button
endef

CONFIGURE_ARGS += \
	--disable-nls \
	--disable-static \
	--enable-shared \
	--with-kerneldir="$(LINUX_DIR)" \
	--with-driver="$(CONFIG_LIRC_DRIVER)" \
	--with-moduledir="$(TARGET_MODULES_DIR)" \
	--with-transmitter \
	--without-x \
	--with-pic \
	--with-gnu-ld \
	--enable-sandboxed \

define Package/lirc/conffiles
/etc/lircd.conf
endef

MAKE_FLAGS += \
	DESTDIR="$(PKG_INSTALL_DIR)" \
	ARCH="$(LINUX_KARCH)" \
	all install

define  Package/lirc/install
	$(INSTALL_DIR) $(1)/etc/
	@if [ -f "$(PKG_INSTALL_DIR)/etc/lircd.conf" ]; then \
		$(INSTALL_CONF) $(PKG_INSTALL_DIR)/etc/lircd.conf $(1)/etc; \
	else \
		$(INSTALL_CONF) $(PKG_BUILD_DIR)/contrib/lircd.conf $(1)/etc; \
	fi
	@if [ -d "$(PKG_INSTALL_DIR)/lib" ]; then \
		$(CP) -r $(PKG_INSTALL_DIR)/lib $(1)/; \
	fi
	$(INSTALL_DIR) $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/lircd $(1)/usr/sbin/
endef

define  Package/lircdaemonadd/install
	$(INSTALL_DIR) $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/daemons/{irrecord,lircmd} $(1)/usr/sbin/
endef

define  Package/lirctools/install
	$(INSTALL_DIR) $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/lircmd $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/mode2 $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/ircat $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/irexec $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/irpty $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/irrecord $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/irsend $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/irw $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/lircrcd $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/mode2 $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/liblirc_client.so* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,lirc))
$(eval $(call BuildPackage,lircdaemonadd))
$(eval $(call BuildPackage,lirctools))
