#
# Copyright (C) 2009-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=krb5
PKG_VERSION:=1.16
PKG_RELEASE:=1

PKG_MAINTAINER:=W. Michael Petullo <mike@flyn.org>

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=NOTICE

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://web.mit.edu/kerberos/dist/krb5/1.16/
PKG_HASH:=faeb125f83b0fb4cdb2f99f088140631bb47d975982de0956d18c85842969e08

PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1
PKG_CHECK_FORMAT_SECURITY:=0

include $(INCLUDE_DIR)/package.mk

MAKE_PATH:=src

define Package/krb5/Default
	SECTION:=net
	CATEGORY:=Network
	TITLE:=Kerberos
	URL:=http://web.mit.edu/kerberos/
endef

define Package/krb5-libs
	SECTION:=net
	CATEGORY:=Network
	TITLE:=Kerberos
	DEPENDS:=+libncurses
	TITLE:=Kerberos 5 Shared Libraries
	URL:=http://web.mit.edu/kerberos/
endef

define Package/krb5-server
	$(call Package/krb5/Default)
	DEPENDS:=+krb5-libs +libpthread
	TITLE:=Kerberos 5 Server
endef

define Package/krb5-client
	$(call Package/krb5/Default)
	DEPENDS:=+krb5-libs
	TITLE:=Kerberos 5 Client
endef

define Package/krb5/description
	Kerberos
endef

CONFIGURE_PATH = ./src

CONFIGURE_VARS += \
	cross_compiling=yes \
	krb5_cv_attr_constructor_destructor=yes,yes \
	ac_cv_func_regcomp=yes \
	ac_cv_printf_positional=yes \
	ac_cv_file__etc_environment=no \
	ac_cv_file__etc_TIMEZONE=no

CONFIGURE_ARGS += \
	--without-system-verto \
	--without-tcl \
	--without-libedit \
	--localstatedir=/etc \
	--with-size-optimizations \
	--disable-rpath \
	--without-krb5-config

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include $(1)/usr
	$(INSTALL_DIR) $(1)/usr
	$(CP) $(PKG_INSTALL_DIR)/usr/lib $(1)/usr
	# needed for samba4, to detect system-krb5
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/krb5-config $(1)/usr/bin
endef

define Package/krb5-libs/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_DIR) $(1)/usr/lib/krb5
	$(INSTALL_DIR) $(1)/usr/lib/krb5/plugins
	$(INSTALL_DIR) $(1)/usr/lib/krb5/plugins/kdb
	$(INSTALL_DIR) $(1)/usr/lib/krb5/plugins/libkrb5
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/krb5/plugins/kdb/db2.so $(1)/usr/lib/krb5/plugins/kdb
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib
endef

define Package/krb5-client/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/kdestroy $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/kinit $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/klist $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/kpasswd $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/ksu $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/kvno $(1)/usr/bin
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/krb5-send-pr $(1)/usr/sbin
endef

# Removed some server-side software to reduce package size. This should be
# put in a separate package if needed.
define Package/krb5-server/install
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/krb5kdc $(1)/etc/init.d/krb5kdc
#	$(INSTALL_DIR) $(1)/usr/bin
#	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/sclient $(1)/usr/bin
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/kadmin.local $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/kadmind $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/kdb5_util $(1)/usr/sbin
#	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/kprop $(1)/usr/sbin
#	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/kpropd $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/krb5kdc $(1)/usr/sbin
#	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/sim_server $(1)/usr/sbin
endef

$(eval $(call BuildPackage,krb5-libs))
$(eval $(call BuildPackage,krb5-server))
$(eval $(call BuildPackage,krb5-client))
