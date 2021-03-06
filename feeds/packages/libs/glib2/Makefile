#
# Copyright (C) 2007-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=glib2
PKG_VERSION:=2.48.1
PKG_RELEASE:=1

PKG_SOURCE:=glib-$(PKG_VERSION).tar.xz
PKG_BUILD_DIR:=$(BUILD_DIR)/glib-$(PKG_VERSION)
PKG_SOURCE_URL:=@GNOME/glib/2.48
PKG_MD5SUM:=67bd3b75c9f6d5587b457dc01cdcd5bb

PKG_BUILD_PARALLEL:=1
HOST_BUILD_PARALLEL:=1
PKG_BUILD_DEPENDS:=libpthread zlib libintl libffi
HOST_BUILD_DEPENDS:=libintl/host libiconv/host libffi/host
PKG_INSTALL:=1
PKG_USE_MIPS16:=0

PKG_FIXUP:=autoreconf

HOST_BUILD_DIR:=$(BUILD_DIR_HOST)/glib-$(PKG_VERSION)

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/glib2
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=$(ICONV_DEPENDS) $(INTL_DEPENDS) +zlib +libpthread +libffi +libattr
  TITLE:=glib 2.0
  MAINTAINER:=Peter Wagner <tripolar@gmx.at>
  URL:=http://www.gtk.org/
endef

define Package/glib2/description
  The GLib library of C routines
endef

HOST_CONFIGURE_ARGS += \
	--disable-selinux --enable-installed-tests=no --enable-always-build-tests=no --enable-gtk-doc-html=no

CONFIGURE_ARGS += \
	--enable-shared \
	--enable-static \
	--enable-debug=no \
	--disable-selinux \
	--enable-installed-tests=no \
	--enable-always-build-tests=no \
	--enable-gtk-doc-html=no \
	--disable-fam \
	--with-pcre=internal \
	$(if $(ICONV_FULL),--with-libiconv=gnu)

CONFIGURE_VARS += \
	glib_cv_stack_grows=no \
	glib_cv_uscore=no \
	ac_cv_path_GLIB_GENMARSHAL=$(STAGING_DIR_HOST)/bin/glib-genmarshal \
	ac_cv_func_mmap_fixed_mapped=yes \
	ac_cv_func_posix_getpwuid_r=yes \
	ac_cv_func_posix_getgrgid_r=yes
	
ifneq ($(CONFIG_TARGET_ZTE),)
CONFIGURE_VARS += glib_cv_eventfd=no
endif
ifneq ($(CONFIG_TARGET_ZTE279121),)
CONFIGURE_VARS += glib_cv_eventfd=no
endif		
ifneq ($(CONFIG_TARGET_ZXIC),)
CONFIGURE_VARS += glib_cv_eventfd=no
endif

ifneq ($(CONFIG_TARGET_BRCM6858),)
CONFIGURE_VARS += LIBFFI_CFLAGS=$(STAGING_DIR_ROOT)/usr/lib
endif



define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/include/glib-2.0 \
		$(1)/usr/include/
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/glib-2.0/include/*.h \
		$(1)/usr/include/glib-2.0/
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/include/gio-unix-2.0 \
		$(1)/usr/include/

	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/glib-2.0 \
		$(1)/usr/lib/

	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/*.{so*,a,la} \
		$(1)/usr/lib/

	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/lib/pkgconfig/*.pc \
		$(1)/usr/lib/pkgconfig

	$(INSTALL_DIR) $(2)/share/aclocal/
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/share/aclocal/*.m4  \
		$(2)/share/aclocal/
endef

define Package/glib2/install
	$(INSTALL_DIR) $(1)/usr/lib
#ifneq ($(CONFIG_TARGET_ZTE),)	
#	$(CP) $(CONFIG_TOOLCHAIN_ROOT)/arm-linux-uclibcgnueabi/lib/libintl.so* $(1)/usr/lib
#	$(CP) $(CONFIG_TOOLCHAIN_ROOT)/arm-linux-uclibcgnueabi/lib/libiconv.so* $(1)/usr/lib
#endif
ifneq ($(CONFIG_TARGET_ZXIC),)	
	$(CP) $(CONFIG_TOOLCHAIN_ROOT)/arm-linux-uclibcgnueabi/lib/libintl.so* $(1)/usr/lib
	$(CP) $(CONFIG_TOOLCHAIN_ROOT)/arm-linux-uclibcgnueabi/lib/libiconv.so* $(1)/usr/lib
endif
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/*.so* \
		$(1)/usr/lib/
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,glib2))
