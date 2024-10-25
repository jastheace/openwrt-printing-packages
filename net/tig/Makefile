#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=tig
PKG_VERSION:=1.2.1
PKG_RELEASE:=1

PKG_SOURCE:=tig-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/jonas/tig/archive/refs/tags/
PKG_MD5SUM:=1cf29accd6a850c7621e5272d5186920
PKG_CAT:=zcat

PKG_BUILD_DIR:=$(BUILD_DIR)/tig-tig-$(PKG_VERSION)
PKG_BUILD_DEPENDS:=ncurses libiconv

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/tig
	SECTION:=devel
	CATEGORY:=Development
	TITLE:=Console Git repository viewer
	DEPENDS:=+git +libncurses
	URL:=https://github.com/jonas/tig/archive/refs/tags/
endef

define Package/tig/description
	Tig is a git repository browser that additionally can act as a pager for output
	from various git commands.
endef

CONFIGURE_ARGS+= \
	--with-libiconv="$(ICONV_PREFIX)"

define Package/tig/install
	$(INSTALL_DIR) $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/tig $(1)/usr/bin/tig
endef

$(eval $(call BuildPackage,tig))
