include $(SUPPORT_DIR)/functions.mk

NAME := libxcb
HOST_DEPENDENCIES := libxau libxdmcp xcb-proto libpthread-stubs
TARGET_DEPENDENCIES := libxau libxdmcp xcb-proto libpthread-stubs
SOURCE := $(SOURCES_DIR)/libxcb-1.12.tar.bz2
SOURCE_URL := https://xcb.freedesktop.org/dist/libxcb-1.12.tar.bz2
MD5 := 28e552bd78bc1050b6b26ca1db0e5bb6

toolchain:
	@$(STEP) "libXcb 1.12"
	@$(call check_source, $(SOURCE), $(MD5), $(SOURCE_URL))
	@$(call toolchain_dependencies, $(HOST_DEPENDENCIES))
	@$(EXTRACT) $(SOURCE) $(BUILD_DIR)
	@( cd $(BUILD_DIR)/libxcb-1.12 && \
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
	--sysconfdir=$(TOOLS_DIR)/etc \
	--localstatedir=$(TOOLS_DIR)/var \
	--enable-shared \
	--disable-static )
	@make -j$(CONFIG_PARALLEL_JOBS) -C $(BUILD_DIR)/libxcb-1.12
	@make -j$(CONFIG_PARALLEL_JOBS) install -C $(BUILD_DIR)/libxcb-1.12
	@rm -rf $(BUILD_DIR)/libxcb-1.12
	@echo $(NAME) >> $(TOOLCHAIN_DEPENDENCIES)

system:
	@export CC=$(TOOLS_DIR)/bin/$(CONFIG_TARGET)-gcc
	@export CXX=$(TOOLS_DIR)/bin/$(CONFIG_TARGET)-g++
	@export AR=$(TOOLS_DIR)/bin/$(CONFIG_TARGET)-ar
	@export AS=$(TOOLS_DIR)/bin/$(CONFIG_TARGET)-as
	@export LD=$(TOOLS_DIR)/bin/$(CONFIG_TARGET)-ld
	@export RANLIB=$(TOOLS_DIR)/bin/$(CONFIG_TARGET)-ranlib
	@export READELF=$(TOOLS_DIR)/bin/$(CONFIG_TARGET)-readelf
	@export STRIP=$(TOOLS_DIR)/bin/$(CONFIG_TARGET)-strip
	@$(STEP) "libXcb 1.12"
	@$(call check_source, $(SOURCE), $(MD5), $(SOURCE_URL))
	@$(call system_dependencies, $(TARGET_DEPENDENCIES))
	@$(EXTRACT) $(SOURCE) $(BUILD_DIR)
	@sed -i "s/pthread-stubs//" $(BUILD_DIR)/libxcb-1.12/configure
	@sed -i "s|XCBPROTO_XCBINCLUDEDIR=|XCBPROTO_XCBINCLUDEDIR=$(SYSROOT_DIR)/|" $(BUILD_DIR)/libxcb-1.12/configure.ac
	@sed -i "s|XCBPROTO_XCBPYTHONDIR=|XCBPROTO_XCBPYTHONDIR=$(SYSROOT_DIR)/|" $(BUILD_DIR)/libxcb-1.12/configure.ac
	@( cd $(BUILD_DIR)/libxcb-1.12 && \
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
	--with-doxygen=no )
	@make -j$(CONFIG_PARALLEL_JOBS) -C $(BUILD_DIR)/libxcb-1.12
	@make -j$(CONFIG_PARALLEL_JOBS) DESTDIR=$(SYSROOT_DIR) install -C $(BUILD_DIR)/libxcb-1.12
	@$(call dependency_libs_patch)
	@make -j$(CONFIG_PARALLEL_JOBS) DESTDIR=$(ROOTFS_DIR) install -C $(BUILD_DIR)/libxcb-1.12
	@rm -rf $(BUILD_DIR)/libxcb-1.12
	@echo $(NAME) >> $(SYSTEM_DEPENDENCIES)
