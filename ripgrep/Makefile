# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2023 Luca Barbato and Donald Hoskins

include $(TOPDIR)/rules.mk

PKG_NAME:=ripgrep
PKG_VERSION:=14.1.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/BurntSushi/ripgrep/tar.gz/$(PKG_VERSION)?
PKG_HASH:=33c6169596a6bbfdc81415910008f26e0809422fda2d849562637996553b2ab6

PKG_MAINTAINER:=Luca Barbato <lu_zero@luminem.org>
PKG_LICENSE:=MIT Unlicense
PKG_LICENSE_FILES:=LICENSE-MIT UNLICENSE
PKG_CPE_ID:=cpe:/a:ripgrep_project:ripgrep

PKG_BUILD_DEPENDS:=rustup/host
PKG_BUILD_PARALLEL:=1

RUST_PKG_FEATURES:=pcre2

include $(INCLUDE_DIR)/package.mk

RUSTC_HOST_ARCH:=$(HOST_ARCH)-unknown-linux-$(CONFIG_HOST_SUFFIX)

CARGO_HOME := $(STAGING_DIR_HOST)/.cargo
RUSTUP_HOME := $(STAGING_DIR_HOST)/.rustup
NIGHTLY_ARGS := -v -Z unstable-options -Z build-std \
		--release \
		--target $(REAL_GNU_TARGET_NAME) \
		--config target.$(REAL_GNU_TARGET_NAME).linker=\"$(TARGET_CC_NOCACHE)\" \
		--config target.$(REAL_GNU_TARGET_NAME).ar=\"$(TARGET_AR)\" \
		--config profile.lto=true

RUST_BUILD_VARS := \
        RUSTUP_HOME="$(RUSTUP_HOME)" \
        CARGO_HOME="$(CARGO_HOME)"

## Force pcre2-sys crate to link dynamically with libpcre2
export PCRE2_SYS_STATIC=0

define Build/Compile
	cd $(PKG_BUILD_DIR) && \
	$(RUST_BUILD_VARS) cargo +nightly build $(NIGHTLY_ARGS)
endef

define Package/ripgrep
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=ripgrep (rg) regex grep
  DEPENDS:=$(RUST_ARCH_DEPENDS) +libpcre2
  URL:=https://github.com/BurntSushi/ripgrep
endef

define Package/ripgrep/description
  ripgrep (rg) recursively searches directories for a regex pattern
  while respecting your gitignore
endef

define Package/ripgrep/install
	$(INSTALL_DIR) $(1)/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/target/$(REAL_GNU_TARGET_NAME)/release/rg $(1)/bin/rg
endef

$(eval $(call BuildPackage,ripgrep))
