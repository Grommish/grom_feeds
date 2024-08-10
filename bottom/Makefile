# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2024 Donald Hoskins <grommish@gmail.com>

include $(TOPDIR)/rules.mk

PKG_NAME:=bottom
PKG_VERSION:=0.9.4
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/ClementTsang/bottom/tar.gz/$(PKG_VERSION)?
PKG_HASH:=199123ef354bcabaa8a2e3b7b477b324f5b647d503a2599d08296733846eea6e

PKG_MAINTAINER:=Donald Hoskins <grommish@gmail.com>
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_PARALLEL:=1

HOST_BUILD_DEPENDS := rustup
RUSTC_HOST_ARCH := $(HOST_ARCH)-unknown-linux-$(CONFIG_HOST_SUFFIX)
RUSTUP_HOME := $(STAGING_DIR_HOST)/.rustup
CARGO_HOME := $(STAGING_DIR_HOST)/.cargo
NIGHTLY_ARGS := -v -Z unstable-options -Z build-std \
                --config target.$(REAL_GNU_TARGET_NAME).linker=\'$(TARGET_CC_NOCACHE)\' \
                --config target.$(REAL_GNU_TARGET_NAME).ar=\'$(TARGET_AR)\' \
                --config profile.lto=true

RUSTFLAGS := "-C linker=$(TARGET_CC_NOCACHE) -C ar=$(TARGET_AR) -C target-feature=-crt-static"

RUST_BUILD_VARS := \
        RUSTUP_HOME="$(RUSTUP_HOME)" \
        CARGO_HOME="$(CARGO_HOME)" \
        CARGO_TARGET_DIR="$(PKG_BUILD_DIR)/rust/target" \
        NIGHTLY_ARGS="$(NIGHTLY_ARGS)"


include $(INCLUDE_DIR)/package.mk

define Package/bottom
  SECTION:=admin
  CATEGORY:=Administration
  TITLE:=Interactive processes viewer
  DEPENDS:=$(RUST_ARCH_DEPENDS)
  URL:=https://github.com/ClementTsang/bottom
endef

define Package/bottom/description
  A customizable cross-platform graphical process/system monitor
  for the terminal.
endef

$(eval $(call BuildPackage,bottom))
