#
# Copyright (C) 2016-2017 LEDE project
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_NAME:=nconf
PKG_NAME:=node-$(PKG_NPM_NAME)
PKG_VERSION:=0.8.4
PKG_RELEASE:=18

PKG_SOURCE:=$(PKG_NPM_NAME)-$(PKG_VERSION).tgz
PKG_SOURCE_URL:=http://registry.npmjs.org/$(PKG_NPM_NAME)/-/
PKG_MD5SUM:=e5736e538d3db55a3fc44edac7d6eb19
PKG_HASH:=974a7e4c33914ad21fa9cfde5cda7c9e18efdd69d7ad82c0acd5893578237736

PKG_BUILD_DEPENDS:=node/host
PKG_USE_MIPS16:=0

PKG_MAINTAINER:=
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk

define Package/node-nconf
  DEPENDS:=+node
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Hierarchical configuration for Node.js
  URL:=https://www.npmjs.org/package/nconf
endef

define Package/node-nconf/description
 Hierarchical node.js configuration with files, environment variables, command-line arguments, and atomic object merging
endef

NODEJS_CPU:=$(subst powerpc,ppc,$(subst aarch64,arm64,$(subst x86_64,x64,$(subst i386,ia32,$(ARCH)))))

define Build/Prepare
	$(INSTALL_DIR) $(PKG_BUILD_DIR)
endef

define Build/Compile
	$(MAKE_VARS) \
	$(MAKE_FLAGS) \
	npm_config_arch=$(NODEJS_CPU) \
	npm_config_nodedir=$(STAGING_DIR_HOSTPKG)/ \
	npm_config_cache=$(STAGING_DIR_HOSTPKG)/share/node/npm-cache \
	PREFIX="$(PKG_INSTALL_DIR)/usr/" \
	npm install --build-from-source --target_arch=$(NODEJS_CPU) -g $(DL_DIR)/$(PKG_SOURCE)
endef

define Package/node-nconf/install
	$(INSTALL_DIR) $(1)/usr/lib/node
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/* $(1)/usr/lib/node/
endef

$(eval $(call BuildPackage,node-nconf))
