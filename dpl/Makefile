#
# Copyright (C) 2020-2024 MaxLinear, Inc.
# Copyright (C) 2020 Intel Corporation
# Copyright (C) 2020 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=dpl
PKG_VERSION:=1.0.32
PKG_RELEASE:=1
PKG_SOURCE_VERSION:=$(PKG_VERSION)
PKG_SOURCE_PROTO:=git
PKG_SOURCE:=$(PKG_NAME)-$(PKG_SOURCE_VERSION).tar.xz
PKG_PROJECT:=maxlinear
PKG_SOURCE_NAME:=dpl
CONFIG_UGW_PKG_SOURCE_URL:=https://github.com
PKG_SOURCE_URL:=$(CONFIG_UGW_PKG_SOURCE_URL)/$(PKG_PROJECT)/$(PKG_SOURCE_NAME).git
PKG_MIRROR_HASH:=7a82fddba1db08e542ba7191b33a32cf4266628f159d7f31f4358c7a4c201732
PKG_MAINTAINER:=MaxLinear
PKG_LICENSE:=GPL-2.0

PKG_BUILD_PARALLEL:=1
PKG_IS_KERNEL_MODULE=y
PKG_DEFAULT_CUSTOM_SOURCE_DIR:=$(TOPDIR)/dpl/

-include $(INCLUDE_DIR)/package-version-override.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/$(PKG_NAME)
  SUBMENU:=MaxLinear
  TITLE:=DoS Protection Layer
  DEPENDS:=@TARGET_intel_x86_lgm                    \
           +kmod-pp_net_adapter                     \

  MAINTAINER:=MaxLinear
  FILES:= $(PKG_BUILD_DIR)/dpl.ko
  AUTOLOAD:=$(call AutoProbe,dpl)
  MENU:=1
endef

define KernelPackage/$(PKG_NAME)/description
  DoS Protection Layer
endef

define KernelPackage/$(PKG_NAME)/config
  $(call Package/kmod-$(PKG_NAME)/override_version)
  $(call Package/kmod-$(PKG_NAME)/override_source_path)
endef

MAKE_OPTS:= \
	$(KERNEL_MAKE_FLAGS) \
	M="$(PKG_BUILD_DIR)"

define Build/Compile
	$(MAKE) -C "$(LINUX_DIR)" \
		$(MAKE_OPTS) \
		EXTRA_CFLAGS="$(EXTRA_CFLAGS)" \
		modules
endef

define Build/Clean
	if [ -d $(PKG_BUILD_DIR) ]; then \
		$(MAKE) -C $(PKG_BUILD_DIR) KERNEL_DIR=$(LINUX_DIR) clean; \
		$(RM) -r $(PKG_BUILD_DIR)/ipkg-$(BOARD); \
	fi
endef

$(eval $(call KernelPackage,$(PKG_NAME)))
