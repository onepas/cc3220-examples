SIMPLELINK_CC32XX_SDK_INSTALL_DIR ?= $(abspath ../../../../../..)

include $(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/imports.mak

CC = "$(GCC_ARMCOMPILER)/bin/arm-none-eabi-gcc"
LNK = "$(GCC_ARMCOMPILER)/bin/arm-none-eabi-gcc"

OBJECTS = uart_term.obj main_nortos.obj mqueue_nortos.obj cloud_ota.obj platform.obj CC3220S_LAUNCHXL.obj

NAME = cloud_ota

CFLAGS = -I.. -DCC32XX "-I$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source" -DNORTOS_SUPPORT "-I$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/kernel/nortos" "-I$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/kernel/nortos/posix" -mcpu=cortex-m4 -march=armv7e-m -mthumb -mfloat-abi=soft -ffunction-sections -fdata-sections -g -gstrict-dwarf -Wall "-I$(GCC_ARMCOMPILER)/arm-none-eabi/include/newlib-nano" "-I$(GCC_ARMCOMPILER)/arm-none-eabi/include"

LFLAGS = "-L$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/net/ota/gcc/Release" -l:ota.a "-L$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/net/json/gcc/Release" -l:json.a -Wl,-T,../gcc/CC3220S_LAUNCHXL_NoRTOS.lds "-Wl,-Map,$(NAME).map" "-L$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/display/lib" -l:display.am4g "-L$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/drivers/lib" -l:drivers_cc32xx.am4g "-L$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/third_party/fatfs/lib" -l:fatfs.am4g -eresetISR "-L$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/drivers/net/wifi/gcc/nortos" -l:simplelink.a "-L$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/kernel/nortos/lib" -l:nortos_cc32xx.am4g "-L$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/devices/cc32xx/driverlib/gcc/Release" -l:driverlib.a -march=armv7e-m -mthumb -nostartfiles -static -Wl,--gc-sections "-L$(GCC_ARMCOMPILER)/arm-none-eabi/lib/thumb/v7e-m" -lgcc -lc -lm -lnosys --specs=nano.specs

all: $(NAME).out

uart_term.obj: ../uart_term.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -c -o $@

main_nortos.obj: ../main_nortos.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -c -o $@

mqueue_nortos.obj: ../mqueue_nortos.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -c -o $@

cloud_ota.obj: ../cloud_ota.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -c -o $@

platform.obj: ../platform.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -c -o $@

CC3220S_LAUNCHXL.obj: ../CC3220S_LAUNCHXL.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -c -o $@

$(NAME).out: $(OBJECTS)
	@ echo linking...
	@ $(LNK)  $(OBJECTS) $(LFLAGS) -o $(NAME).out

clean:
	@ echo Cleaning...
	@ $(RM) $(OBJECTS) > $(DEVNULL) 2>&1
	@ $(RM) $(NAME).out > $(DEVNULL) 2>&1
	@ $(RM) $(NAME).map > $(DEVNULL) 2>&1
