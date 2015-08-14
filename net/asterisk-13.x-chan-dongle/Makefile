#
# Copyright (C) 2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=asterisk13-chan-dongle
PKG_VERSION:=1.1r35
PKG_REV:=53ed602dbcc5ce482309c03ef96366ff60ee4b25
PKG_RELEASE:=4

PKG_SOURCE_SUBDIR:=asterisk13-chan-dongle-$(PKG_VERSION)
PKG_SOURCE:=$(PKG_SOURCE_SUBDIR).tar.gz
PKG_SOURCE_URL:=/home/zhiqiang/asterisk-chan-dongle/.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_REV)

PKG_BUILD_DIR=$(BUILD_DIR)/$(PKG_SOURCE_SUBDIR)

PKG_FIXUP:=autoreconf

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYRIGHT.txt LICENSE.txt
PKG_MAINTAINER:=Jiri Slachta <slachta@cesnet.cz>

include $(INCLUDE_DIR)/package.mk

define Package/asterisk13-chan-dongle
  SUBMENU:=Telephony
  SECTION:=net
  CATEGORY:=Network
  URL:=https://code.google.com/p/asterisk-chan-dongle/
  DEPENDS:= asterisk13 +libiconv-full +kmod-usb-acm +kmod-usb-serial +kmod-usb-serial-option +libusb-1.0 +usb-modeswitch
  TITLE:=Huawei UMTS 3G dongle support
endef

define Package/asterisk13-chan-dongle/description
 Asterisk channel driver for Huawei UMTS 3G dongle.
endef

MAKE_ARGS:= \
	CC="$(TARGET_CC)" \
	LD="$(TARGET_CC)" \
	CFLAGS="$(TARGET_CFLAGS) -Wno-builtin-macro-redefined -DASTERISK_VERSION_NUM=130000 -DLOW_MEMORY -D_XOPEN_SOURCE=600 -DHAVE_CONFIG_H $(TARGET_CPPFLAGS) -I$(STAGING_DIR)/usr/lib/libiconv-full/include -I$(STAGING_DIR)/usr/include/asterisk-13/include -I. -fPIC" \
	LDFLAGS="$(TARGET_LDFLAGS) -L$(STAGING_DIR)/usr/lib/libiconv-full/lib -liconv" \
	DESTDIR="$(PKG_INSTALL_DIR)/usr/lib/asterisk/modules"

CONFIGURE_VARS += \
	ac_cv_type_size_t=yes \
	ac_cv_type_ssize_t=yes

define Build/Configure
	$(call Build/Configure/Default, \
	    --with-asterisk=$(STAGING_DIR)/usr/include/asterisk-13/include \
	    $(MAKE_ARGS) \
	)
endef

define Build/Compile
	mkdir -p $(PKG_INSTALL_DIR)/usr/lib/asterisk/modules
	$(MAKE) -C "$(PKG_BUILD_DIR)" $(MAKE_ARGS) all install
endef

define Package/asterisk13-chan-dongle/conffiles
/etc/asterisk/dongle.conf
endef

define Package/asterisk13-chan-dongle/install
	$(INSTALL_DIR) $(1)/etc/asterisk
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/etc/dongle.conf $(1)/etc/asterisk/
	$(INSTALL_DIR) $(1)/usr/lib/asterisk/modules
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/asterisk/modules/chan_dongle.so $(1)/usr/lib/asterisk/modules/
endef

$(eval $(call BuildPackage,asterisk13-chan-dongle))
