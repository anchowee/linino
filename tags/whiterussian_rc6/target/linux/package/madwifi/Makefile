# $Id$

include $(TOPDIR)/rules.mk

PKG_NAME:=madwifi
PKG_VERSION:=r1208-20060106
PKG_RELEASE:=1
PKG_MD5SUM:=d475d75d87d3d4ee2f6e8f905f7d0acc

PKG_SOURCE_URL:=http://snapshots.madwifi.org/madwifi-old
PKG_SOURCE:=$(PKG_NAME)-old-$(PKG_VERSION).tar.gz
PKG_CAT:=zcat

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-old-$(PKG_VERSION)

ifeq ($(KERNEL_DIR),)
KERNEL_DIR:=$(LINUX_DIR)
endif

ifeq ($(ARCH),mips)
HAL_TARGET:=mips-be-elf
endif
ifeq ($(ARCH),mipsel)
HAL_TARGET:=mips-le-elf
endif
ifeq ($(ARCH),i386)
HAL_TARGET:=i386-elf
endif

include $(TOPDIR)/package/rules.mk

$(eval $(call PKG_template,KMOD_MADWIFI,kmod-madwifi,$(LINUX_VERSION)$(BOARD)+$(PKG_VERSION)-$(PKG_RELEASE),$(ARCH),kernel ($(LINUX_VERSION)-$(BOARD)-$(KERNEL_RELEASE))))

$(PKG_BUILD_DIR)/.configured:
	touch $@

$(PKG_BUILD_DIR)/.built:
	$(MAKE) -C $(PKG_BUILD_DIR) \
		TARGET="$(HAL_TARGET)" \
		TOOLPREFIX="$(KERNEL_CROSS)" \
		KERNELPATH="$(KERNEL_DIR)"
	touch $@

$(IPKG_KMOD_MADWIFI):
	mkdir -p $(IDIR_KMOD_MADWIFI)/etc/modules.d
	mkdir -p $(IDIR_KMOD_MADWIFI)/etc/init.d
	mkdir -p $(IDIR_KMOD_MADWIFI)/lib/modules/$(LINUX_VERSION)
	install -m0644 ./files/madwifi.modules $(IDIR_KMOD_MADWIFI)/etc/modules.d/20-madwifi
	install -m0755 ./files/madwifi.init $(IDIR_KMOD_MADWIFI)/etc/init.d/S20madwifi
	cp -fpR $(PKG_BUILD_DIR)/net80211/wlan*.o \
		$(PKG_BUILD_DIR)/ath_hal/ath_hal.o \
		$(PKG_BUILD_DIR)/ath_rate/sample/ath_rate_sample.o \
		$(PKG_BUILD_DIR)/ath/ath_pci.o \
		$(IDIR_KMOD_MADWIFI)/lib/modules/$(LINUX_VERSION)/
	$(IPKG_BUILD) $(IDIR_KMOD_MADWIFI) $(PACKAGE_DIR)

