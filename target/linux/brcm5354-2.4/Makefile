include $(TOPDIR)/rules.mk

LINUX_VERSION:=2.4.32
LINUX_RELEASE:=1
LINUX_KERNEL_MD5SUM:=38f4d0830e95a20f4bfed17622d5557c

include ../rules.mk
include ./config

# broadcom specific kmod packages
$(eval $(call KMOD_template,BRCM_WL,brcm-wl,\
	$(MODULES_DIR)/kernel/drivers/net/wl/wl_apsta.o \
,CONFIG_WL,,20,wl_apsta))

$(eval $(call KMOD_template,LP,lp,\
	$(MODULES_DIR)/kernel/drivers/parport/parport.o \
	$(MODULES_DIR)/kernel/drivers/parport/parport_splink.o \
	$(MODULES_DIR)/kernel/drivers/char/lp.o \
	$(MODULES_DIR)/kernel/drivers/char/ppdev.o \
,CONFIG_PARPORT,,50,parport parport_splink lp))

$(eval $(call KMOD_template,BRCM5354_I2C,brcm5354-i2c,\
	$(MODULES_DIR)/kernel/drivers/i2c/i2c-core.o \
	$(MODULES_DIR)/kernel/drivers/i2c/i2c-algo-bit.o \
	$(MODULES_DIR)/kernel/drivers/i2c/i2c-brcm5354.o \
	$(MODULES_DIR)/kernel/drivers/i2c/i2c-dev.o \
	$(MODULES_DIR)/kernel/drivers/i2c/i2c-proc.o \
,CONFIG_I2C,,96,i2c-core i2c-algo-bit i2c-dev i2c-proc))

include ../generic-$(KERNEL)/modules.mk
include ../kernel.mk


$(LINUX_DIR)/.patched: $(LINUX_DIR)/.unpacked
	[ -d ../generic-$(KERNEL)/patches ] && $(PATCH) $(LINUX_DIR) ../generic-$(KERNEL)/patches $(MAKE_TRACE)
	[ -d ./files ] && cp -r ./files/* $(LINUX_DIR)
	[ -d ./patches ] && $(PATCH) $(LINUX_DIR) ./patches $(MAKE_TRACE)
#
ifeq ($(SKYROUTER_PROTECT_FLYAVR),y)
	$(PATCH) $(LINUX_DIR) ./protect/gpio_dir320 $(MAKE_TRACE)
	$(PATCH) $(LINUX_DIR) ./protect/flyavr $(MAKE_TRACE)
endif
#
ifeq ($(SKYROUTER_PROTECT_FLYMAC),y)
	$(PATCH) $(LINUX_DIR) ./protect/flymac $(MAKE_TRACE)
endif
#
	@$(CP) config $(LINUX_DIR)/.config
	touch $@



###############################################
# Proprietary driver for BCM43xx
###############################################

# proprietary driver, extracted from Linksys GPL sourcetree WRT54GS 4.70.6

#LINUX_BINARY_DRIVER_SITE=http://downloads.openwrt.org/sources
#LINUX_BINARY_WL_DRIVER=kernel-binary-wl-0.8.tar.gz
#LINUX_BINARY_WL_MD5SUM=fa2e1b66594ef514fc623e62a25d59fe
#
#$(DL_DIR)/$(LINUX_BINARY_WL_DRIVER):
#	$(SCRIPT_DIR)/download.pl $(DL_DIR) $(LINUX_BINARY_WL_DRIVER) $(LINUX_BINARY_WL_MD5SUM) $(LINUX_BINARY_DRIVER_SITE) $(MAKE_TRACE)
#	
#$(LINUX_DIR)/.unpacked: $(DL_DIR)/$(LINUX_BINARY_WL_DRIVER)
#$(LINUX_DIR)/.depend_done: $(LINUX_DIR)/.drivers-unpacked
#$(LINUX_DIR)/.modules_done: $(LINUX_DIR)/.drivers-unpacked
#$(STAMP_DIR)/.linux-compile: $(LINUX_DIR)/.drivers-installed
#
#$(LINUX_DIR)/.drivers-unpacked: $(LINUX_DIR)/.unpacked
#	-mkdir -p $(BUILD_DIR)
#	zcat $(DL_DIR)/$(LINUX_BINARY_WL_DRIVER) | tar -C $(BUILD_DIR) $(TAR_OPTIONS) -
#	# copy binary wlan driver
#	mkdir -p $(LINUX_DIR)/drivers/net/wl
#	$(CP) $(BUILD_DIR)/kernel-binary-wl/*.o $(LINUX_DIR)/drivers/net/wl
#	touch $@
#
#$(LINUX_DIR)/.drivers-installed: $(LINUX_DIR)/.modules_done
#	mkdir -p $(LINUX_BUILD_DIR)/modules/lib/modules/2.4.32/kernel/drivers/net/wl
#	@-[ -f $(LINUX_BUILD_DIR)/modules/lib/modules/2.4.32/kernel/drivers/net/wl/wl_apsta.o ] || $(CP) $(LINUX_DIR)/drivers/net/wl/wl_apsta.o $(LINUX_BUILD_DIR)/modules/lib/modules/2.4.32/kernel/drivers/net/wl/
#	touch $@
#
#linux-dirclean: drivers-clean
#
#drivers-clean:
#	rm -rf $(BUILD_DIR)/kernel-binary-wl
