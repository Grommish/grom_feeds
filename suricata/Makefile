include $(TOPDIR)/rules.mk

PKG_NAME:=suricata
PKG_VERSION:=7.0.6
PKG_RELEASE:=1
PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=LICENSE

PKG_SOURCE := $(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL := https://www.openinfosecfoundation.org/download/
PKG_HASH := skip
PKG_BUILD_DEPENDS := rustup/host jansson
#PKG_FIXUP := patch-libtool
#PKG_FIXUP := autoreconf

include $(INCLUDE_DIR)/package.mk

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

CONFIGURE_VARS += \
	$(RUST_BUILD_VARS)

CONFIGURE_ARGS += \
	--build=$(GNU_HOST_NAME) \
	--host=$(REAL_GNU_TARGET_NAME) \
	--prefix=$(PKG_INSTALL_DIR) \
	--with-sysroot=$(TOOLCHAIN_DIR)

define Build/Compile
	$(RM) $(PKG_BUILD_DIR)/rust/.cargo/config.toml

	cd $(PKG_BUILD_DIR) && \
	   $(RUST_BUILD_VARS) make
endef

#define Build/Install
#	cd $(PKG_BUILD_DIR) && \
#	   $(RUST_BUILD_VARS) make install
#
#	cd $(PKG_BUILD_DIR) && \
#	   $(RUST_BUILD_VARS) make install-conf
#endef

define Package/suricata
    SECTION:=testing
    CATEGORY:=Testing
    DEPENDS:=@!SMALL_FLASH @!LOW_MEMORY_FOOTPRINT +libpcre2 +libyaml +jansson +libpcap +libcap-ng +libunwind \
             +file +liblz4 +libunwind
    TITLE:=Suricata 7
    URL:=https://github.com/OISF/suricata
endef

define Package/suricata/description
  Suricata 7
endef

define Package/suricata/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/suricata $(1)/usr/bin/suricata
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/suricatactl $(1)/usr/bin/suricatactl
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/suricatasc $(1)/usr/bin/suricatasc
#
#	$(INSTALL_DIR) $(1)/usr/lib
#	$(CP) -r $(PKG_INSTALL_DIR)/usr/lib/* $(1)/usr/lib/
#
#	$(INSTALL_DIR) $(1)/usr/include
#	$(CP) -r $(PKG_INSTALL_DIR)/usr/include/* $(1)/usr/include/
#
#	$(INSTALL_DIR) $(1)/etc/suricata
#	$(CP) $(PKG_BUILD_DIR)/suricata.yaml \
#	$(PKG_BUILD_DIR)/etc/classification.config \
#	$(PKG_BUILD_DIR)/threshold.config \
#	$(PKG_BUILD_DIR)/etc/reference.config \
#	$(1)/etc/suricata/
#
#	$(INSTALL_DIR) $(1)/usr/share/suricata/rules
#	$(CP) $(PKG_INSTALL_DIR)/usr/share/suricata/rules/* $(1)/usr/share/suricata/rules/
#
#	$(INSTALL_DIR) $(1)/etc/init.d
#	$(INSTALL_DIR) $(1)/etc/config
#
#	$(INSTALL_BIN) ./files/etc/init.d/suricata $(1)/etc/init.d/suricata
#	$(INSTALL_CONF) ./files/etc/config/suricata $(1)/etc/config/suricata
endef

$(eval $(call BuildPackage,suricata))
