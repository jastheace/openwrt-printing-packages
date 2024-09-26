include $(TOPDIR)/rules.mk

PKG_NAME:=libz
PKG_VERSION:=v1.2.11
PKG_RELEASE:=1

PKG_BUILD_DIR:=$(BUILD_DIR)/jbigkit-$(PKG_VERSION)
PKG_SOURCE:=$(PKG_VERSION)/libz-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://gitlab.com/sortix/libz/-/archive
PKG_MD5SUM:=ebcf09bed9f14d7fa188d3bd57349522
PKG_CAT:=zcat

include $(INCLUDE_DIR)/package.mk

define Package/libz
	SECTION:=libs
	CATEGORY:=Libraries
	TITLE:=libz library
	#DESCRIPTION:=This variable is obsolete. use the Package/name/description define instead!
	DEPENDS:=
	URL:=https://gitlab.com/sortix/libz/
endef

define Package/libjbigkit/description
	libz is a general purpose data compression library. The data format used by libz is described by RFC 1950 (zlib format), RFC 1951 (deflate format), and RFC 1952 (gzip format)
endef

define Package/libz/install
	$(INSTALL_DIR) $(1)/usr/bin $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/* $(1)/usr/lib/
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/ijs $(1)/usr/lib
	$(CP) -r $(PKG_INSTALL_DIR)/usr/include/ijs $(1)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/* $(1)/usr/lib
endef

$(eval $(call BuildPackage,libz))
