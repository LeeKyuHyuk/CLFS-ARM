include $(SUPPORT_DIR)/functions.mk

SOURCE := $(SOURCES_DIR)/help2man-1.47.5.tar.xz
SOURCE_URL := http://ftpmirror.gnu.org/gnu/help2man/help2man-1.47.5.tar.xz
MD5 := 1c886063c9e0ab463906de804596aa2f

toolchain:
	@$(STEP) "help2man 1.47.5"
	@$(call check_source, $(SOURCE), $(MD5), $(SOURCE_URL))
	@$(EXTRACT) $(SOURCE) $(BUILD_DIR)
	@( cd $(BUILD_DIR)/help2man-1.47.5 && \
	PKG_CONFIG="$(TOOLS_DIR)/bin/pkg-config" \
	PKG_CONFIG_SYSROOT_DIR="/" \
	PKG_CONFIG_ALLOW_SYSTEM_CFLAGS=1 \
	PKG_CONFIG_ALLOW_SYSTEM_LIBS=1 \
	PKG_CONFIG_LIBDIR="$(TOOLS_DIR)/lib/pkgconfig:$(TOOLS_DIR)/share/pkgconfig" \
	CPPFLAGS="-I$(TOOLS_DIR)/include" \
	CFLAGS="-O2 -I$(TOOLS_DIR)/include" \
	CXXFLAGS="-O2 -I$(TOOLS_DIR)/include" \
	LDFLAGS="-L$(TOOLS_DIR)/lib -Wl,-rpath,$(TOOLS_DIR)/lib" \
	MAKEINFO=true \
	CONFIG_SITE=/dev/null \
	./configure \
	--prefix=$(TOOLS_DIR) \
	--sysconfdir=$(TOOLS_DIR)/etc \
	--localstatedir=$(TOOLS_DIR)/var \
	--enable-shared \
	--disable-static )
	@make -j$(CONFIG_PARALLEL_JOBS) -C $(BUILD_DIR)/help2man-1.47.5
	@make -j$(CONFIG_PARALLEL_JOBS) install -C $(BUILD_DIR)/help2man-1.47.5
	@rm -rf $(BUILD_DIR)/help2man-1.47.5
