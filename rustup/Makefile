# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2023 Luca Barbato and Donald Hoskins

include $(TOPDIR)/rules.mk

PKG_NAME:=rustup
PKG_VERSION:=1.26.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/rust-lang/rustup/
PKG_SOURCE_VERSION:=5af9b9484c1bb7a747700af5940ff9c479801dc8
PKG_MIRROR_HASH:=skip
PKG_LICENSE_FILES:=LICENSE-APACHE LICENSE-MIT
PKG_SOURCE:=rustup-$(PKG_VERSION).tar.gz
PKG_MAINTAINER:=Donald Hoskins <grommish@gmail.com>

PKG_HOST_ONLY:=1
PKG_BUILD_FLAGS:=no-mips16

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk
include rust-values.mk

define Package/rustup
  SECTION:=lang
  CATEGORY:=Languages
  SUBMENU:=Rustup
  TITLE:=Rust Programming Language Compiler
  URL:=https://www.rust-lang.org/
  DEPENDS:=$(RUST_ARCH_DEPENDS)
endef

define Package/rustup/description
  Rustup is the toolchain manager for Rust-Lang
endef


# Arguments Used:
# --verbose Verbosity
# -y Disable the Confirmation Prompts
# --profile complete Profile to install [default: default] [possible values: minimal, default, complete]
# --no-modify-path Don't configure the PATH environment variable
# --default-host <default-host> Choose a default host triple
# --default-toolchain <default-toolchain> Choose a default toolchain to install. Use 'none' to not install any toolchains at all
RUSTUP_ARGS = \
	--verbose \
	-y \
	--profile complete \
	--no-modify-path \
	--default-host $(RUSTUP_HOST_ARCH) \
	--default-toolchain nightly

RUSTUP_BINARIES = \
	cargo \
	cargo-clippy \
	cargo-fmt \
	cargo-miri \
	clippy-driver \
	rls \
	rust-analyzer \
	rust-gdb \
	rust-gdbgui \
	rust-lldb \
	rustc \
	rustdoc \
	rustfmt \
	rustup

define Host/Uninstall
	$(RM) -r $(CARGO_HOME)
	$(RM) -r $(RUSTUP_HOME)
	$(foreach file,$(RUSTUP_BINARIES),$(RM) $(STAGING_DIR_HOST)/bin/$(file);)
endef

define Host/Compile
	CARGO_HOME=$(CARGO_HOME) \
	RUSTUP_HOME=$(RUSTUP_HOME) \
	$(BASH) $(HOST_BUILD_DIR)/rustup-init.sh $(RUSTUP_ARGS)
endef

define Host/Install
	$(foreach file,$(RUSTUP_BINARIES),ln -s $(CARGO_HOME)/bin/$(file) $(STAGING_DIR_HOST)/bin/$(file);)
	cd $(STAGING_DIR_HOST)/bin && \
	  CARGO_HOME=$(CARGO_HOME) \
	  RUSTUP_HOME=$(RUSTUP_HOME) \
	  $(STAGING_DIR_HOST)/bin/rustup +nightly component add rust-src --toolchain nightly-$(RUSTUP_HOST_ARCH)
endef

$(eval $(call HostBuild))
#$(eval $(call BuildPackage,rustup))
