#
# Copyright (C) 2006-2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=cups
PKG_VERSION:=2.4.12
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-source.tar.gz
PKG_SOURCE_URL:=https://github.com/OpenPrinting/cups/releases/download/v$(PKG_VERSION)/
PKG_MD5SUM:=1f9281c661b95acc7925a220919210e2

TARGET_LDFLAGS+=-Wl,-rpath-link=$(STAGING_DIR)/usr/lib

include $(INCLUDE_DIR)/package.mk

define Package/cups/Default
  URL:=http://www.cups.org/
  SUBMENU:=Printing
endef

define Package/cups
$(call Package/cups/Default)
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+zlib +libusb-1.0 +libopenssl +libavahi-client +libavahi-dbus-support +libpng +libjpeg +libstdcpp +libwrap #+libopenldap +libpam +libavahi-dbus-support
  TITLE:=Common UNIX Printing System (daemon)
  SUBMENU:=Printing
endef

define Package/cups/description
	Common UNIX Printing System (daemon)
endef

define Package/cups/conffiles
/etc/cups/classes.conf
/etc/cups/cupsd.conf
/etc/cups/printers.conf
endef

define Build/Configure
	$(call Build/Configure/Default, \
		--with-cups-user="nobody" \
		--with-cups-group="nogroup" \
		--enable-default-shared \
		--enable-shared \
		--enable-image \
		--enable-libusb \
		--enable-tcp-wrappers \
		--enable-browsing \
		--enable-webif \
		--disable-acl \
		--enable-dbus \
		--with-dnssd=avahi \
		--disable-pam \
		--disable-static \
		--disable-gnutls \
		--enable-openssl \
		--disable-gssapi \
		UNAME="Linux" \
		LIBS="$(TARGET_LDFLAGS) -lz -lpng -ljpeg" \
	)
endef

CONFIGURE_ARGS+=
MAKE_FLAGS = \
        ARCH="$(LINUX_KARCH)" \
        CROSS_COMPILE="$(TARGET_CROSS)" \
        CC="$(TARGET_CC)" \
        LD="$(TARGET_CROSS)ld" \
        EXTRA_CFLAGS="$(TARGET_CFLAGS) $(TARGET_CPPFLAGS)" \
        LDFLAGS="$(TARGET_LDFLAGS)" \
        KBUILD_HOSTCFLAGS="$(HOST_CFLAGS)" \
        PKG_CONFIG="$(PKG_CONFIG)" \
        PKG_CONFIG_PATH="$(PKG_CONFIG_PATH)"

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		$(TARGET_CONFIGURE_OPTS) \
		DSTROOT="$(PKG_INSTALL_DIR)" \
		STRIP="/bin/true" \
		all install
	$(INSTALL_DIR) $(PKG_INSTALL_DIR)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/cups-config $(PKG_INSTALL_DIR)/usr/bin
	# Comment out any mime.convs using CUPS' own filters. These
	# are deprecated by OpenPrinting's cups-filters package. Perl
	# command taken from cups-filters-1.0.X/INSTALL
	perl -p -i -e 's:^(.*\s+(pdftops|texttops|imagetops|bannertops|imagetoraster)\s*)$$$$:#\1:' $(PKG_INSTALL_DIR)/usr/share/cups/mime/mime.convs
endef

define Package/cups/install
	
	#$(INSTALL_DIR) $(1)/etc/init.d
	#$(CP) $(PKG_INSTALL_DIR)/etc/init.d/* $(1)/etc/init.d/
	
	rm -rf $(1)/etc/cups
	$(INSTALL_DIR) $(1)/etc/cups
	$(CP) $(PKG_INSTALL_DIR)/etc/cups/* $(1)/etc/cups/
	rm -rf $(1)/etc/cups/certs
	ln -sf /tmp $(1)/etc/cups/certs
	rm -f $(1)/usr/bin/cups-config
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so.2 $(1)/usr/lib/

	$(INSTALL_DIR) $(1)/usr/lib/cups
	### OpenPrinting cups-filters --------------------------------
	rm -f $(PKG_INSTALL_DIR)/usr/lib/cups/backend/parallel
	rm -f $(PKG_INSTALL_DIR)/usr/lib/cups/backend/serial
	###-----------------------------------------------------------
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cups/backend $(1)/usr/lib/cups
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cups/cgi-bin $(1)/usr/lib/cups
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cups/daemon $(1)/usr/lib/cups
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cups/driver $(1)/usr/lib/cups
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cups/monitor $(1)/usr/lib/cups
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cups/command $(1)/usr/lib/cups
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cups/notifier $(1)/usr/lib/cups
	$(INSTALL_DIR) $(1)/usr/lib/cups/filter
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cups/filter/* $(1)/usr/lib/cups/filter/
	
	$(INSTALL_DIR) $(1)/usr/share/cups
	$(CP) $(PKG_INSTALL_DIR)/usr/share/cups/* $(1)/usr/share/cups
	$(INSTALL_DIR) $(1)/usr/share/cups/templates 
	$(CP) $(PKG_INSTALL_DIR)/usr/share/cups/templates/*.tmpl \
		$(1)/usr/share/cups/templates/
	$(INSTALL_DIR) $(1)/usr/share/cups/mime
	$(CP) $(PKG_INSTALL_DIR)/usr/share/cups/mime/* $(1)/usr/share/cups/mime/
	### Do not install any docs.
	### Do install docs: cups web interface relies on them
	$(INSTALL_DIR) $(1)/usr/share/doc/cups
	$(CP) $(PKG_INSTALL_DIR)/usr/share/doc/cups/*.*html \
		$(1)/usr/share/doc/cups/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/doc/cups/*.css \
		$(1)/usr/share/doc/cups/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/doc/cups/*.txt \
		$(1)/usr/share/doc/cups/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/doc/cups/images \
		$(1)/usr/share/doc/cups/
	$(INSTALL_DIR) $(1)/usr/sbin
	$(CP) $(PKG_INSTALL_DIR)/usr/sbin/* $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin/

	# overwrite default config with our own
	$(CP) ./files/etc/cups/* $(1)/etc/cups/
	# install initscript with priority 60
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/cupsd.init $(1)/etc/init.d/cupsd
#	$(INSTALL_BIN) \
	# needed for cups to find usb printers per http://wiki.openwrt.org/doc/howto/cups.server
	chmod 700 $(1)/usr/lib/cups/backend/usb
endef


define Build/InstallDev
	$(INSTALL_DIR) $(2)/bin $(1)/usr/bin $(1)/usr/include $(1)/usr/lib $(1)/usr/lib/pkgconfig
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/cups-config $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/include/cups $(1)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib*/libcups*.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/cups.pc $(1)/usr/lib/pkgconfig/
	$(SED) 's,^\(prefix\|exec_prefix\)=.*,\1=$(STAGING_DIR)/usr,g' $(1)/usr/bin/cups-config
	[ -n "$(TARGET_LDFLAGS)" ] && $(SED) 's#$(TARGET_LDFLAGS)##g' $(1)/usr/lib/pkgconfig/cups.pc || true
	$(LN) $(STAGING_DIR)/usr/bin/cups-config $(2)/bin/
endef

$(eval $(call BuildPackage,cups))
