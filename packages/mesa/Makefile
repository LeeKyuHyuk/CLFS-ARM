include $(SUPPORT_DIR)/functions.mk

NAME := mesa
TARGET_DEPENDENCIES := expat zlib libdrm
SOURCE := $(SOURCES_DIR)/mesa-17.2.2.tar.xz
SOURCE_URL := https://mesa.freedesktop.org/archive/mesa-17.2.2.tar.xz
MD5 := 1a157b5baefb5adf9f4fbb8a6632d74c

system:
	@export CC=$(TOOLS_DIR)/bin/$(CONFIG_TARGET)-gcc
	@export CXX=$(TOOLS_DIR)/bin/$(CONFIG_TARGET)-g++
	@export AR=$(TOOLS_DIR)/bin/$(CONFIG_TARGET)-ar
	@export AS=$(TOOLS_DIR)/bin/$(CONFIG_TARGET)-as
	@export LD=$(TOOLS_DIR)/bin/$(CONFIG_TARGET)-ld
	@export RANLIB=$(TOOLS_DIR)/bin/$(CONFIG_TARGET)-ranlib
	@export READELF=$(TOOLS_DIR)/bin/$(CONFIG_TARGET)-readelf
	@export STRIP=$(TOOLS_DIR)/bin/$(CONFIG_TARGET)-strip
	@$(STEP) "mesa 17.2.2"
	@$(call check_source, $(SOURCE), $(MD5), $(SOURCE_URL))
	@$(call system_dependencies, $(TARGET_DEPENDENCIES))
	@$(EXTRACT) $(SOURCE) $(BUILD_DIR)
	@( cd $(BUILD_DIR)/mesa-17.2.2 && autoconf )
	@( cd $(BUILD_DIR)/mesa-17.2.2 && \
	PKG_CONFIG="$(TOOLS_DIR)/bin/pkg-config" \
	ac_cv_lbl_unaligned_fail=yes \
	ac_cv_func_mmap_fixed_mapped=yes \
	ac_cv_func_memcmp_working=yes \
	ac_cv_have_decl_malloc=yes \
	gl_cv_func_malloc_0_nonnull=yes \
	ac_cv_func_malloc_0_nonnull=yes \
	ac_cv_func_calloc_0_nonnull=yes \
	ac_cv_func_realloc_0_nonnull=yes \
	lt_cv_sys_lib_search_path_spec="" \
	ac_cv_c_bigendian=no \
	CONFIG_SITE=/dev/null \
	./configure \
	--target=$(CONFIG_TARGET) \
	--host=$(CONFIG_TARGET) \
	--build=$(CONFIG_HOST) \
	--prefix=/usr \
	--exec-prefix=/usr \
	--sysconfdir=/etc \
	--localstatedir=/var \
	--program-prefix="" \
	--disable-static \
	--enable-shared \
	--disable-asm \
	--enable-glx \
	--disable-mangling \
	--enable-texture-float \
	--enable-osmesa \
	--enable-xa \
	--enable-glx-tls \
	--enable-shared-glapi \
	--with-gallium-drivers=swrast,vc4 \
	--enable-gallium-extra-hud \
	--without-dri-drivers \
	--disable-dri3 \
	--without-vulkan-drivers \
	--disable-osmesa \
	--enable-opengl \
	--enable-dri \
	--disable-va \
	--with-platforms=drm,x11 \
	--enable-gbm \
	--enable-egl \
	--enable-gles1 \
	--enable-gles2 \
	--disable-texture-float \
	--disable-xvmc \
	--disable-libunwind \
	--disable-vdpau \
	--disable-lmsensors \
	--with-llvm-prefix=$(SYSROOT_DIR)/usr/bin )
	@make -j$(CONFIG_PARALLEL_JOBS) -C $(BUILD_DIR)/mesa-17.2.2
	@make -j$(CONFIG_PARALLEL_JOBS) DESTDIR=$(SYSROOT_DIR) install -C $(BUILD_DIR)/mesa-17.2.2
	@$(call dependency_libs_patch)
	@make -j$(CONFIG_PARALLEL_JOBS) DESTDIR=$(ROOTFS_DIR) install -C $(BUILD_DIR)/mesa-17.2.2
	@rm -rf $(BUILD_DIR)/mesa-17.2.2
	@echo $(NAME) >> $(SYSTEM_DEPENDENCIES)
