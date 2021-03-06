# 
# Copyright (C) 2006 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/image.mk

UCODEFILE:=IPL_ixp400NpeLibrary-2_4.zip

ifneq ($(CONFIG_IXP4XX_INCLUDE_UCODE),)
  define Require/npe-ucode
	[ -f $(DL_DIR)/$(UCODEFILE) ]
  endef
  define Build/Compile/npe
	$(MAKE) -C npe-ucode \
		BUILD_DIR="$(KDIR)" \
		TARGET="$(KDIR)" \
		compile
  endef
  define Image/Build/slug
	BIN_DIR=$(BIN_DIR) $(TOPDIR)/scripts/slugimage.pl \
		-L $(KDIR)/apex-nslu2-armeb.bin -m $(KDIR)/NPE-B \
		-k $(BIN_DIR)/openwrt-nslu2-$(KERNEL)-zImage \
		-r rootfs:$(BIN_DIR)/openwrt-$(BOARD)-$(KERNEL)-$(1).img \
		-p -o $(BIN_DIR)/openwrt-nslu2-$(KERNEL)-$(1).bin
  endef
endif

define Build/Compile
	$(MAKE) -C apex \
		BUILD_DIR="$(KDIR)" \
		TARGET="$(KDIR)" \
		compile	
	$(call Build/Compile/npe)
endef

define Build/Clean
	$(MAKE) -C apex clean
	$(MAKE) -C npe-ucode clean
endef

define Image/Prepare
	cp $(LINUX_DIR)/arch/arm/boot/zImage $(KDIR)/zImage
endef

define Image/BuildKernel
	cp $(KDIR)/zImage $(BIN_DIR)/openwrt-$(BOARD)-$(KERNEL)-zImage
#	$(shell BIN_DIR=$(BIN_DIR) $(TOPDIR)/scripts/arm-magic.sh)
	BIN_DIR=$(BIN_DIR) $(TOPDIR)/scripts/arm-magic.sh
endef

define Image/Build
	$(call Image/Build/$(1),$(1))
endef

define Image/Build/jffs2-64k
	dd if=$(KDIR)/root.$(1) of=$(BIN_DIR)/openwrt-$(BOARD)-$(KERNEL)-$(1).img bs=65536 conv=sync
endef

define Image/Build/jffs2-128k
	dd if=$(KDIR)/root.$(1) of=$(BIN_DIR)/openwrt-$(BOARD)-$(KERNEL)-$(1).img bs=131072 conv=sync
	$(call Image/Build/slug,$(1))
endef

define Image/Build/squashfs
    $(call prepare_generic_squashfs,$(KDIR)/root.squashfs)
	dd if=$(KDIR)/root.$(1) of=$(BIN_DIR)/openwrt-$(BOARD)-$(KERNEL)-$(1).img bs=131072 conv=sync
	$(call Image/Build/slug,$(1))
endef

$(eval $(call BuildImage))

$(eval $(call Require,npe-ucode, \
	You must manually download $(UCODEFILE) from \\\
	http://downloadcenter.intel.com/detail_desc.aspx?ProductID=2100&DwnldID=12954&agr=Y \\\
	and put it in $(DL_DIR). \\\
	You will need to agree to the Intel Public License to do so - please do read it! \
))
