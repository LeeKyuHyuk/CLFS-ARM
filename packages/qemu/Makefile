include $(SUPPORT_DIR)/functions.mk

NAME := qemu
HOST_DEPENDENCIES := vde2 sdl2
SOURCE := $(SOURCES_DIR)/qemu-2.10.0.tar.xz
SOURCE_URL := https://download.qemu.org/qemu-2.10.0.tar.xz
MD5 := a89e3293cf69c32cf4be4188dfa25544

toolchain:
	@$(STEP) "qemu 2.10.0"
	@$(call check_source, $(SOURCE), $(MD5), $(SOURCE_URL))
	@$(call toolchain_dependencies, $(HOST_DEPENDENCIES))
	@$(EXTRACT) $(SOURCE) $(BUILD_DIR)
	@( cd $(BUILD_DIR)/qemu-2.10.0 && \
	PKG_CONFIG="$(TOOLS_DIR)/bin/pkg-config" \
	PKG_CONFIG_SYSROOT_DIR="/" \
	PKG_CONFIG_ALLOW_SYSTEM_CFLAGS=1 \
	PKG_CONFIG_ALLOW_SYSTEM_LIBS=1 \
	PKG_CONFIG_LIBDIR="$(TOOLS_DIR)/lib/pkgconfig:$(TOOLS_DIR)/share/pkgconfig" \
	CPPFLAGS="-I$(TOOLS_DIR)/include" \
	CFLAGS="-O2 -I$(TOOLS_DIR)/include" \
	CXXFLAGS="-O2 -I$(TOOLS_DIR)/include" \
	LDFLAGS="-L$(TOOLS_DIR)/lib -Wl,-rpath,$(TOOLS_DIR)/lib" \
	CONFIG_SITE=/dev/null \
	./configure \
	--prefix=$(TOOLS_DIR) \
	--interp-prefix=$(SYSROOT_DIR) \
	--target-list="aarch64-softmmu,arm-softmmu,aarch64-linux-user,arm-linux-user,armeb-linux-user" \
	--cc="gcc" \
	--host-cc="gcc" \
	--python=$(TOOLS_DIR)/bin/python2 \
	--extra-cflags="-O2 -I$(TOOLS_DIR)/include" \
	--extra-ldflags="-L$(TOOLS_DIR)/lib -Wl,-rpath,$(TOOLS_DIR)/lib" \
	--enable-system \
	--enable-fdt \
	--enable-linux-user \
	--enable-vde \
	--enable-sdl )
	@make -j$(CONFIG_PARALLEL_JOBS) -C $(BUILD_DIR)/qemu-2.10.0
	@make -j$(CONFIG_PARALLEL_JOBS) install -C $(BUILD_DIR)/qemu-2.10.0
	@rm -rf $(BUILD_DIR)/qemu-2.10.0
	@echo $(NAME) >> $(TOOLCHAIN_DEPENDENCIES)
