SIMPLELINK_CC32XX_SDK_INSTALL_DIR ?= $(abspath ../../../../../../..)

include $(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/imports.mak

KERNEL_BUILD := $(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/kernel/freertos/builds/CC3220S_LAUNCHXL/release

CC = "$(GCC_ARMCOMPILER)/bin/arm-none-eabi-gcc"
LNK = "$(GCC_ARMCOMPILER)/bin/arm-none-eabi-gcc"

OBJECTS = uart_term.obj main_freertos.obj CC3220S_LAUNCHXL.obj link_local_task.obj local_ota.obj local_ota_task.obj ota_archive.obj ota_json.obj ota_report_server_task.obj platform.obj

NAME = local_ota

CFLAGS = -I../.. -DCC32XX "-I$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source" "-I$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/kernel/freertos/posix" "-I$(FREERTOS_INSTALL_DIR)/FreeRTOS/Source/include" "-I$(FREERTOS_INSTALL_DIR)/FreeRTOS/Source/portable/GCC/ARM_CM3" "-I$(KERNEL_BUILD)" -mcpu=cortex-m4 -march=armv7e-m -mthumb -mfloat-abi=soft -ffunction-sections -fdata-sections -g -gstrict-dwarf -Wall "-I$(GCC_ARMCOMPILER)/arm-none-eabi/include"

LFLAGS = "-L$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/net/json/gcc/Release" -l:json.a -Wl,-T,../../freertos/gcc/CC3220S_LAUNCHXL_FREERTOS.lds "-Wl,-Map,$(NAME).map" "-L$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/display/lib" -l:display.am4g "-L$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/drivers/lib" -l:drivers_cc32xx.am4g "-L$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/third_party/fatfs/lib" -l:fatfs.am4g "-L$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/drivers/net/wifi/gcc/rtos" -l:simplelink.a "-L$(KERNEL_BUILD)/gcc" -l:freertos.lib "-L$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/devices/cc32xx/driverlib/gcc/Release" -l:driverlib.a -march=armv7e-m -mthumb -nostartfiles -static -Wl,--gc-sections -lgcc -lc -lm -lnosys

all: $(NAME).out

$(KERNEL_BUILD)/gcc/freertos.lib:
	@ $(ECHOBLANKLINE)
	@ echo $@ is not built.
	@ echo You can build it by issuing $(MAKE) in $(dir $@).
	@ $(ECHOBLANKLINE)

uart_term.obj: ../../uart_term.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -c -o $@

main_freertos.obj: ../../freertos/main_freertos.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -c -o $@

CC3220S_LAUNCHXL.obj: ../../CC3220S_LAUNCHXL.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -c -o $@

link_local_task.obj: ../../link_local_task.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -c -o $@

local_ota.obj: ../../local_ota.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -c -o $@

local_ota_task.obj: ../../local_ota_task.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -c -o $@

ota_archive.obj: ../../ota_archive.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -c -o $@

ota_json.obj: ../../ota_json.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -c -o $@

ota_report_server_task.obj: ../../ota_report_server_task.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -c -o $@

platform.obj: ../../platform.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -c -o $@

$(NAME).out: $(OBJECTS) $(KERNEL_BUILD)/gcc/freertos.lib
	@ echo linking...
	@ $(LNK)  $(OBJECTS) $(LFLAGS) -o $(NAME).out

clean:
	@ echo Cleaning...
	@ $(RM) $(OBJECTS) > $(DEVNULL) 2>&1
	@ $(RM) $(NAME).out > $(DEVNULL) 2>&1
	@ $(RM) $(NAME).map > $(DEVNULL) 2>&1
