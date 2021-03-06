#
# Copyright (C) 2016-2017 LEDE project
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_NAME:=babel
PKG_NAME:=node-$(PKG_NPM_NAME)
PKG_VERSION:=v6.25.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/babel/babel.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=82f37841f54a69efe70acd5b1b8b142a102908c8
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_MIRROR_HASH:=9f2b2158ca3475784970cbc2f69625162c2f08bfcac2514f5b1a40c99f6fc853

PKG_BUILD_DEPENDS:=node/host
PKG_USE_MIPS16:=0

PKG_MAINTAINER:=
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

define Package/node-babel/default
  DEPENDS:=+node
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=BABEL $(1) for JavaScript
  URL:=https://babeljs.io/
endef

define Package/node-babel
  $(call Package/node-babel/default,cli)
endef

define Package/node-babel-preset-latest
  $(call Package/node-babel/default,preset-latest)
endef

define Package/node-babel/description
 The compiler for writing next generation JavaScript
endef

NODEJS_CPU:=$(subst powerpc,ppc,$(subst aarch64,arm64,$(subst x86_64,x64,$(subst i386,ia32,$(ARCH)))))

define Build/Compile/module
	$(MAKE_VARS) \
	$(MAKE_FLAGS) \
	npm_config_arch=$(NODEJS_CPU) \
	npm_config_nodedir=$(STAGING_DIR_HOSTPKG)/ \
	npm_config_cache=$(STAGING_DIR_HOSTPKG)/share/node/npm-cache \
	PREFIX="$(PKG_INSTALL_DIR)/usr/" \
	npm install --build-from-source --target_arch=$(NODEJS_CPU) -g $(PKG_BUILD_DIR)/packages/$(1)/
endef

define Build/Compile
	$(call Build/Compile/module,babel-cli)
	$(call Build/Compile/module,babel-preset-latest)
endef

define Package/node-babel/install
	$(INSTALL_DIR) $(1)/usr/lib/node/babel-cli
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/babel-cli/* $(1)/usr/lib/node/babel-cli/
	$(INSTALL_DIR) $(1)/usr/bin
	$(LN) ../lib/node/babel-cli/bin/babel.js $(1)/usr/bin/babel
	$(LN) ../lib/node/babel-cli/bin/babel-doctor.js $(1)/usr/bin/babel-doctor
	$(LN) ../lib/node/babel-cli/bin/babel-external-helpers.js $(1)/usr/bin/babel-external-helpers
	$(LN) ../lib/node/babel-cli/bin/babel-node.js $(1)/usr/bin/babel-node
endef

define Package/node-babel-preset-latest/install
	$(INSTALL_DIR) $(1)/usr/lib/node/babel-preset-latest
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/babel-preset-latest/* $(1)/usr/lib/node/babel-preset-latest/
endef

define Host/Compile/module
	npm_config_nodedir=$(STAGING_DIR_HOSTPKG)/ \
	npm_config_cache=$(STAGING_DIR_HOSTPKG)/share/node/npm-cache \
	PREFIX="$(STAGING_DIR_HOSTPKG)/" \
	npm install -g $(HOST_BUILD_DIR)/packages/$(1)/
endef

define Host/Compile
	$(call Host/Compile/module,babel-cli)
	$(call Host/Compile/module,babel-preset-latest)
endef

define Host/Install
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,node-babel))
$(eval $(call BuildPackage,node-babel-preset-latest))
