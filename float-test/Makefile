include $(TOPDIR)/rules.mk

PKG_NAME:=float-test
PKG_VERSION:=0.8.5
PKG_RELEASE:=1
PKG_LICENSE:=Apache-2.0 MIT
PKG_LICENSE_FILES:=LICENSE-APACHE LICENSE-MIT

PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2021-06-12
PKG_SOURCE_VERSION:=245dd4b5b6ec3b99855b452b052d887171b76665
PKG_SOURCE_URL:=https://github.com/Grommish/float_test.git
PKG_MIRROR_HASH:=skip

PKG_BUILD_DEPENDS:=rustup/host

include $(INCLUDE_DIR)/package.mk

RUSTC_HOST_ARCH:=$(HOST_ARCH)-unknown-linux-$(CONFIG_HOST_SUFFIX)

CARGO_HOME := $(STAGING_DIR_HOST)/.cargo
RUSTUP_HOME := $(STAGING_DIR_HOST)/.rustup
RUSTFLAGS := "-C linker=$(TARGET_CC_NOCACHE) -C ar=$(TARGET_AR)"

CARGO_MAKEFLAGS := "-Z unstable-options -Z build-std"
RUST_BUILD_VARS := \
        RUSTUP_HOME="$(RUSTUP_HOME)" \
        CARGO_HOME="$(CARGO_HOME)" \
        RUSTFLAGS=$(RUSTFLAGS)

define Build/Compile
	cd $(PKG_BUILD_DIR) && \
	  $(RUST_BUILD_VARS) cargo +nightly build -Z unstable-options -Z build-std -v --release --target $(REAL_GNU_TARGET_NAME)
endef

define Package/float-test
    SECTION:=testing
    CATEGORY:=Testing
    DEPENDS:=@!SMALL_FLASH @!LOW_MEMORY_FOOTPRINT
    TITLE:=float-test
    URL:=https://github.com/Grommish
endef

define Package/float-test/description
  Rust-base application to test floating point math
endef

define Package/float-test/install
	$(INSTALL_DIR) $(1)/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/target/$(REAL_GNU_TARGET_NAME)/release/float_test $(1)/bin/float_test

endef

$(eval $(call BuildPackage,float-test))
