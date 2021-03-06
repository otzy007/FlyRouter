# $Id: Makefile 3112 2006-02-01 23:53:19Z mbm $

include $(TOPDIR)/rules.mk

PKG_NAME := kmod-wlcompat
PKG_RELEASE := 3
PKG_BUILD_DIR := $(BUILD_DIR)/$(PKG_NAME)

include $(TOPDIR)/package/rules.mk

$(eval $(call PKG_template,KMOD_WLCOMPAT,$(PKG_NAME),$(LINUX_VERSION)-$(BOARD)-$(PKG_RELEASE),$(ARCH),kernel ($(LINUX_VERSION)-$(BOARD)-$(LINUX_RELEASE))))
$(eval $(call PKG_template,KMOD_WLCOMPAT_DEBUG,$(PKG_NAME)-debug,$(LINUX_VERSION)-$(BOARD)-$(PKG_RELEASE),$(ARCH),kernel ($(LINUX_VERSION)-$(BOARD)-$(LINUX_RELEASE))))

WLCOMPAT_FLAGS:=$(TARGET_CFLAGS) -D__KERNEL__ -fno-strict-aliasing -fno-common -fomit-frame-pointer -G 0 \
		-mno-abicalls -fno-pic -finline-limit=100000 -mabi=32 -march=mips32 -Wa,-32 -Wa,-march=mips32 \
		-Wa,-mips32 -Wa,--trap -DMODULE -mlong-calls -fno-common -I. -I linux-2.4 -funsigned-char -nostdinc \
		-iwithprefix include -I$(LINUX_DIR)/include -I$(LINUX_DIR)/include/asm/gcc -I$(STAGING_DIR)/usr/include -c

$(PKG_BUILD_DIR)/.prepared:
	mkdir -p $(PKG_BUILD_DIR)
	touch $@

$(PKG_BUILD_DIR)/.built: $(PKG_BUILD_DIR)/wlcompat.o $(PKG_BUILD_DIR)/wlcompat-debug.o
	touch $@
	
$(PKG_BUILD_DIR)/wlcompat.o: $(PKG_BUILD_DIR)/.prepared
	$(TARGET_CC) $(WLCOMPAT_FLAGS) -o $@ wlcompat.c

$(PKG_BUILD_DIR)/wlcompat-debug.o: $(PKG_BUILD_DIR)/.prepared
	$(TARGET_CC) -DDEBUG $(WLCOMPAT_FLAGS) -o $@ wlcompat.c

$(IPKG_KMOD_WLCOMPAT): $(PKG_BUILD_DIR)/wlcompat.o
	mkdir -p $(IDIR_KMOD_WLCOMPAT)/etc/modules.d
	echo "wlcompat" > $(IDIR_KMOD_WLCOMPAT)/etc/modules.d/30-wlcompat
	mkdir -p $(IDIR_KMOD_WLCOMPAT)/lib/modules/$(LINUX_VERSION)
	$(CP) $(PKG_BUILD_DIR)/wlcompat.o $(IDIR_KMOD_WLCOMPAT)/lib/modules/$(LINUX_VERSION)/
	$(IPKG_BUILD) $(IDIR_KMOD_WLCOMPAT) $(PACKAGE_DIR)

$(IPKG_KMOD_WLCOMPAT_DEBUG): $(PKG_BUILD_DIR)/wlcompat-debug.o
	mkdir -p $(IDIR_KMOD_WLCOMPAT_DEBUG)/lib/modules/$(LINUX_VERSION)
	$(CP) $(PKG_BUILD_DIR)/wlcompat-debug.o $(IDIR_KMOD_WLCOMPAT_DEBUG)/lib/modules/$(LINUX_VERSION)/
	$(IPKG_BUILD) $(IDIR_KMOD_WLCOMPAT_DEBUG) $(PACKAGE_DIR)

