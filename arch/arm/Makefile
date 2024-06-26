#
# arch/arm/Makefile
#
# This file is included by the global makefile so that you can add your own
# architecture-specific flags and dependencies.
#
# This file is subject to the terms and conditions of the GNU General Public
# License.  See the file "COPYING" in the main directory of this archive
# for more details.
#
# Copyright (C) 1995-2001 by Russell King

# Ensure linker flags are correct
LDFLAGS		:=

# Platform Names

platname-$(CONFIG_BOARD_OMAP34XX) 		+= "OMAP3"
platname-$(CONFIG_BOARD_OMAP54XX) 		+= "OMAP5"
platname-$(CONFIG_BOARD_AM335X) 		+= "AM335X"
platname-$(CONFIG_BOARD_ARM_REALVIEW) 		+= "RealView"
platname-$(CONFIG_BOARD_HPTOUCHPAD)		+= "HP-TouchPad"
platname-$(CONFIG_BOARD_HTC_HD2)		+= "HTC-HD2"
platname-$(CONFIG_BOARD_BCM2836)		+= "BCM2836"
platname-$(CONFIG_BOARD_IMX53_USBARMORY)	+= "IMX53-USBarmory"
platname-$(CONFIG_BOARD_SAMSUNG_JF)		+= "Galaxy-S4"

export platname-y := $(platname-y)

KBUILD_CFLAGS	+=-fstack-protector -DBUILD_STYLE=\"DEVELOPMENT\" -DBUILD_TAG=\"$(shell git describe --dirty)\" \
				-DPLATFORM_NAME=$(platname-y)

# Little endian
KBUILD_CPPFLAGS	+= -mlittle-endian
AS		+= -EL
LD		+= -EL -nostdlib

comma = ,

# Processor type
arch-$(CONFIG_PROC_CORTEX_A15)           =-D__LINUX_ARM_ARCH__=7 $(call cc-option,-march=armv7-a,-march=armv5t -Wa$(comma)-march=armv7-a)
arch-$(CONFIG_PROC_CORTEX_A9)		=-D__LINUX_ARM_ARCH__=7 $(call cc-option,-march=armv7-a,-march=armv5t -Wa$(comma)-march=armv7-a)
arch-$(CONFIG_PROC_CORTEX_A8)		=-D__LINUX_ARM_ARCH__=7 $(call cc-option,-march=armv7-a,-march=armv5t -Wa$(comma)-march=armv7-a)
arch-$(CONFIG_PROC_CORTEX_A7)		=-D__LINUX_ARM_ARCH__=7 $(call cc-option,-march=armv7-a,-march=armv5t -Wa$(comma)-march=armv7-a)
arch-$(PROC_ARM1176JZFS)		=-D__LINUX_ARM_ARCH__=6 $(call cc-option,-march=armv6,-march=armv5t -Wa$(comma)-march=armv6)

# Evaluate arch cc-option calls now
arch-y := $(arch-y)

textofs-y 				:= 0x00000040
textofs-$(CONFIG_BOARD_OMAP34XX) 	:= 0x84000040
textofs-$(CONFIG_BOARD_OMAP54XX) 	:= 0x84000040
textofs-$(CONFIG_BOARD_AM335X) 		:= 0x84000040
textofs-$(CONFIG_BOARD_ARM_REALVIEW)	:= 0x84000040
textofs-$(CONFIG_BOARD_HPTOUCHPAD)	:= 0x44000040
textofs-$(CONFIG_BOARD_HTC_HD2)		:= 0x11808000
textofs-$(CONFIG_BOARD_BCM2836)		:= 0x00008000
textofs-$(CONFIG_BOARD_IMX53_USBARMORY)	:= 0x74000040
textofs-$(CONFIG_BOARD_SAMSUNG_JF)      := 0x80208000

export TEXT_OFFSET := $(textofs-y)

# Soft float.
KBUILD_CFLAGS	+=$(arch-y) -msoft-float -Uarm
KBUILD_AFLAGS	+=$(arch-y) -msoft-float

all: uImage

KBUILD_IMAGE = uImage
PHONY += uImage

archprepare:
	$(Q)$(MAKE) $(build)=images Images

uImage: vmlinux
	# Create image.
	@echo Generating uImage...
	@$(OBJCOPY) -g -S -O binary vmlinux vmlinux.raw
	@mkimage -A arm -O linux -T kernel -C none -a $(TEXT_OFFSET) -e $(TEXT_OFFSET) -n "GenericBooter" -d vmlinux.raw uImage
	# rm -f vmlinux.raw

# Boards
core-$(CONFIG_BOARD_OMAP54XX) 		+= arch/arm/plat-omap5/
core-$(CONFIG_BOARD_OMAP34XX) 		+= arch/arm/plat-omap3/
core-$(CONFIG_BOARD_AM335X) 		+= arch/arm/plat-am335x/
core-$(CONFIG_BOARD_ARM_REALVIEW) 	+= arch/arm/plat-realview/
core-$(CONFIG_BOARD_HPTOUCHPAD)		+= arch/arm/plat-hptouchpad/
core-$(CONFIG_BOARD_HTC_HD2)		+= arch/arm/plat-htchd2/
core-$(CONFIG_BOARD_BCM2836)		+= arch/arm/plat-bcm2836/
core-$(CONFIG_BOARD_IMX53_USBARMORY)	+= arch/arm/plat-imx53usbarmory/
core-$(CONFIG_BOARD_SAMSUNG_JF)		+= arch/arm/plat-samsungjf/

core-y				+= arch/arm/common/
