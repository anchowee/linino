#
# Copyright (C) 2008 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# $Id: Makefile 13786 2008-12-31 15:02:25Z lars $

include $(TOPDIR)/rules.mk

PKG_NAME:=python-edje

PKG_REV:=40853
PKG_VERSION:=r$(PKG_REV)
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=svn
PKG_SOURCE_VERSION:=$(PKG_REV)
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://svn.enlightenment.org/svn/e/trunk/BINDINGS/python/$(PKG_NAME)

PKG_BUILD_DEPENDS:=cython python

include $(INCLUDE_DIR)/package.mk
-include $(if $(DUMP),,$(STAGING_DIR)/mk/python-package.mk)

define Package/python-edje
  SUBMENU:=Python
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=python-edje
  DEPENDS:=+python-mini +edje +eina +python-ecore +python-evas
endef

define Package/python-edje/description
  edje python bindings
endef

define Build/Compile
	$(call Build/Compile/PyMod,,install --prefix="$(PKG_INSTALL_DIR)/usr")
endef

define Package/python-edje/install
	$(INSTALL_DIR) $(1)$(PYTHON_PKG_DIR)
	$(CP) \
	    $(PKG_INSTALL_DIR)$(PYTHON_PKG_DIR)/* \
	    $(1)$(PYTHON_PKG_DIR)
endef

$(eval $(call BuildPackage,python-edje))
