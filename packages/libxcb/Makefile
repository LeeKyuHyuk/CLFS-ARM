include $(SUPPORT_DIR)/functions.mk

SOURCE := $(SOURCES_DIR)/libxcb-1.12.tar.bz2
SOURCE_URL := https://xcb.freedesktop.org/dist/libxcb-1.12.tar.bz2
MD5 := 28e552bd78bc1050b6b26ca1db0e5bb6

staging:
	@export CC=$(TOOLS_DIR)/usr/bin/$(CONFIG_TARGET)-gcc
	@export CXX=$(TOOLS_DIR)/usr/bin/$(CONFIG_TARGET)-g++
	@export AR=$(TOOLS_DIR)/usr/bin/$(CONFIG_TARGET)-ar
	@export AS=$(TOOLS_DIR)/usr/bin/$(CONFIG_TARGET)-as
	@export LD=$(TOOLS_DIR)/usr/bin/$(CONFIG_TARGET)-ld
	@export RANLIB=$(TOOLS_DIR)/usr/bin/$(CONFIG_TARGET)-ranlib
	@export READELF=$(TOOLS_DIR)/usr/bin/$(CONFIG_TARGET)-readelf
	@export STRIP=$(TOOLS_DIR)/usr/bin/$(CONFIG_TARGET)-strip
	@$(STEP) "libxcb 1.12"
	@$(call check_source, $(SOURCE), $(MD5), $(SOURCE_URL))
	@$(EXTRACT) $(SOURCE) $(BUILD_DIR)
	@sed -i "s/pthread-stubs//" $(BUILD_DIR)/libxcb-1.12/configure
	@( cd $(BUILD_DIR)/libxcb-1.12 && \
	./configure \
	--target=$(CONFIG_TARGET) \
	--host=$(CONFIG_TARGET) \
	--build=$(CONFIG_HOST) \
 	--prefix=$(SYSROOT_DIR)/usr \
	--sysconfdir=$(SYSROOT_DIR)/etc \
	--localstatedir=$(SYSROOT_DIR)/var \
	--disable-static \
	--enable-shared \
	--with-doxygen=no )
	@make -j$(CONFIG_PARALLEL_JOBS) -C $(BUILD_DIR)/libxcb-1.12
	@make -j$(CONFIG_PARALLEL_JOBS) install -C $(BUILD_DIR)/libxcb-1.12
	@rm -rf $(BUILD_DIR)/libxcb-1.12

system:
	@export CC=$(TOOLS_DIR)/usr/bin/$(CONFIG_TARGET)-gcc
	@export CXX=$(TOOLS_DIR)/usr/bin/$(CONFIG_TARGET)-g++
	@export AR=$(TOOLS_DIR)/usr/bin/$(CONFIG_TARGET)-ar
	@export AS=$(TOOLS_DIR)/usr/bin/$(CONFIG_TARGET)-as
	@export LD=$(TOOLS_DIR)/usr/bin/$(CONFIG_TARGET)-ld
	@export RANLIB=$(TOOLS_DIR)/usr/bin/$(CONFIG_TARGET)-ranlib
	@export READELF=$(TOOLS_DIR)/usr/bin/$(CONFIG_TARGET)-readelf
	@export STRIP=$(TOOLS_DIR)/usr/bin/$(CONFIG_TARGET)-strip
	@$(STEP) "libxcb 1.12"
	@$(call check_source, $(SOURCE), $(MD5), $(SOURCE_URL))
	@$(EXTRACT) $(SOURCE) $(BUILD_DIR)
	@sed -i "s/pthread-stubs//" $(BUILD_DIR)/libxcb-1.12/configure
	@( cd $(BUILD_DIR)/libxcb-1.12 && \
	./configure \
	--target=$(CONFIG_TARGET) \
	--host=$(CONFIG_TARGET) \
	--build=$(CONFIG_HOST) \
 	--prefix=/usr \
	--sysconfdir=/etc \
	--localstatedir=/var \
	--disable-static \
	--enable-shared \
	--with-doxygen=no )
	@make -j$(CONFIG_PARALLEL_JOBS) -C $(BUILD_DIR)/libxcb-1.12
	@make -j$(CONFIG_PARALLEL_JOBS) DESTDIR=$(ROOTFS_DIR) install -C $(BUILD_DIR)/libxcb-1.12
	@rm -rf $(BUILD_DIR)/libxcb-1.12