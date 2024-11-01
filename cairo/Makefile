include $(TOPDIR)/rules.mk

PKG_NAME:=cairo
PKG_VERSION:=1.17.4
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=https://cairographics.org/snapshots/
PKG_HASH:=74b24c1ed436bbe87499179a3b27c43f4143b8676d8ad237a6fa787401959705

PKG_MAINTAINER:=Daniel Golle <daniel@makrotopia.org>
PKG_LICENSE:=LGPL-2.1-or-later MPL-1.1
PKG_LICENSE_FILES:=COPYING COPYING-LGPL-2.1 COPYING-MPL-1.1

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libcairo
  SECTION:=libs
  CATEGORY:=Libraries
  SUBMENU:=Video
  TITLE:=cairo
  URL:=https://cairo.freedesktop.org/
  DEPENDS:=+libpng +libfreetype +fontconfig +pixman +glib2 +libjpeg +zlib +liblzo
endef

define Package/libcairo/description
  Cairo is a 2D graphics library with support for multiple output devices.
endef

CONFIGURE_ARGS += \
	--oldincludedir="$(STAGING_DIR)/usr/include" \
	--x-includes="$(STAGING_DIR)/usr/include" \
	--x-libraries="$(STAGING_DIR)/usr/lib" \
	--without-x \
	--enable-pdf \
	--enable-png \
	--enable-ps \
	--enable-svg \
	--enable-tee \
	--enable-ft \
	--disable-glesv2 \
	--disable-glesv3 \
	--enable-test-surfaces=no \
	--enable-xcb=no \
	--enable-xlib-xrender=no \
	--enable-xcb=no \
	--enable-xlib-xcb=no \
	--enable-xcb-shm=no

define Package/libcairo/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib
	$(INSTALL_DIR) $(1)/usr/lib/cairo
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cairo/*.so $(1)/usr/lib/cairo
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/cairo
	$(CP) $(PKG_INSTALL_DIR)/usr/include/cairo/*.h $(1)/usr/include/cairo
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib
	$(INSTALL_DIR) $(1)/usr/lib/cairo
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cairo/*.so $(1)/usr/lib/cairo
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/*.pc $(1)/usr/lib/pkgconfig
endef

$(eval $(call BuildPackage,libcairo))
