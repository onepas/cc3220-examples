SIMPLELINK_CC32XX_SDK_INSTALL_DIR ?= $(abspath ../../../../../../..)

include $(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/imports.mak

XDCPATH = $(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source;$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/kernel/tirtos/packages;

CC = "$(GCC_ARMCOMPILER)/bin/arm-none-eabi-gcc"
LNK = "$(GCC_ARMCOMPILER)/bin/arm-none-eabi-gcc"

XDCTARGET = gnu.targets.arm.M4
PLATFORM = ti.platforms.simplelink:CC3220S

OBJECTS = benchloop.obj CC3220S_LAUNCHXL.obj

CONFIGPKG = benchloop

NAME = benchloop

CFLAGS = -I../.. "-I$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source" "-I$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/kernel/tirtos/packages/ti/sysbios/posix" -mcpu=cortex-m4 -march=armv7e-m -mthumb -mfloat-abi=soft -ffunction-sections -fdata-sections -g -gstrict-dwarf -Wall "-I$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/kernel/tirtos/packages/gnu/targets/arm/libs/install-native/arm-none-eabi/include/newlib-nano"

LFLAGS = -Wl,-T,../../tirtos/gcc/CC3220S_LAUNCHXL_TIRTOS.lds "-Wl,-Map,$(NAME).map" "-Wl,-T,$(NAME)/linker.cmd" "-L$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/display/lib" -l:display.am4g "-L$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/drivers/lib" -l:drivers_cc32xx.am4g "-L$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/third_party/fatfs/lib" -l:fatfs.am4g "-L$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/kernel/tirtos/packages/ti/dpl/lib" -l:dpl_cc32xx.am4g "-L$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/devices/cc32xx/driverlib/gcc/Release" -l:driverlib.a -march=armv7e-m -mthumb -nostartfiles -static -Wl,--gc-sections "-L$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/kernel/tirtos/packages/gnu/targets/arm/libs/install-native/arm-none-eabi/lib/thumb/v7e-m" -lgcc -lc -lm -lnosys --specs=nano.specs

.PRECIOUS: $(CONFIGPKG)/compiler.opt $(CONFIGPKG)/linker.cmd

all: $(NAME).out

$(CONFIGPKG)/compiler.opt: $(CONFIGPKG)/linker.cmd

$(CONFIGPKG)/linker.cmd: ../../tirtos/benchloop.cfg
	@ echo Running Configuro...
	@ $(XDC_INSTALL_DIR)/xs --xdcpath="$(XDCPATH)" xdc.tools.configuro -c "$(GCC_ARMCOMPILER)" -t $(XDCTARGET) -p $(PLATFORM) --compileOptions "$(CFLAGS)" -o "$(CONFIGPKG)" ../../tirtos/benchloop.cfg

benchloop.obj: ../../benchloop.c $(CONFIGPKG)/compiler.opt
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -c @$(CONFIGPKG)/compiler.opt -o $@

CC3220S_LAUNCHXL.obj: ../../CC3220S_LAUNCHXL.c $(CONFIGPKG)/compiler.opt
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -c @$(CONFIGPKG)/compiler.opt -o $@

$(NAME).out: $(OBJECTS) $(CONFIGPKG)/linker.cmd
	@ echo linking...
	@ $(LNK)  $(OBJECTS)  $(LFLAGS) -o $(NAME).out

clean:
	@ echo Cleaning...
	@ $(RM) $(OBJECTS) > $(DEVNULL) 2>&1
	@ $(RM) $(NAME).out > $(DEVNULL) 2>&1
	@ $(RM) $(NAME).map > $(DEVNULL) 2>&1
	@ $(RMDIR) $(NAME) > $(DEVNULL) 2>&1
