SIMPLELINK_CC32XX_SDK_INSTALL_DIR ?= $(abspath ../../../../../../..)

include $(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/imports.mak

KERNEL_BUILD := $(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/kernel/tirtos/builds/CC3220S_LAUNCHXL/release

CC = "$(IAR_ARMCOMPILER)/bin/iccarm"
LNK = "$(IAR_ARMCOMPILER)/bin/ilinkarm"

OBJECTS = uart_term.obj main_tirtos.obj cloud_ota.obj platform.obj CC3220S_LAUNCHXL.obj

CONFIGPKG = $(KERNEL_BUILD)/iar

NAME = cloud_ota

CFLAGS = -I../.. -DCC32XX "-I$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source" "-I$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/kernel/tirtos/packages/ti/sysbios/posix" --debug --silent -e --aeabi --thumb --diag_suppress=Pa050

LFLAGS = "$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/net/ota/iar/Release/ota.a" "$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/net/json/iar/Release/json.a" "$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/display/lib/display.arm4" "$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/drivers/lib/drivers_cc32xx.arm4" "$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/third_party/fatfs/lib/fatfs.arm4" "$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/drivers/net/wifi/iar/rtos/simplelink.a" "$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/kernel/tirtos/packages/ti/dpl/lib/dpl_cc32xx.arm4" -f "$(KERNEL_BUILD)/iar/linker.cmd" "$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/devices/cc32xx/driverlib/iar/Release/driverlib.a" --config ../../tirtos/iar/CC3220S_LAUNCHXL_TIRTOS.icf --map "$(NAME).map" --silent --cpu=Cortex-M4 --redirect _Printf=_PrintfSmall --redirect _Scanf=_ScanfSmall --semihosting=iar_breakpoint

all: $(NAME).out

$(CONFIGPKG)/linker.cmd $(CONFIGPKG)/compiler.opt:
	@ $(ECHOBLANKLINE)
	@ echo $(CONFIGPKG) is not built.
	@ echo You can build it by issuing $(MAKE) in $(CONFIGPKG).
	@ $(ECHOBLANKLINE)

uart_term.obj: ../../uart_term.c $(CONFIGPKG)/compiler.opt
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -f $(CONFIGPKG)/compiler.opt -o $@

main_tirtos.obj: ../../tirtos/main_tirtos.c $(CONFIGPKG)/compiler.opt
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -f $(CONFIGPKG)/compiler.opt -o $@

cloud_ota.obj: ../../cloud_ota.c $(CONFIGPKG)/compiler.opt
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -f $(CONFIGPKG)/compiler.opt -o $@

platform.obj: ../../platform.c $(CONFIGPKG)/compiler.opt
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -f $(CONFIGPKG)/compiler.opt -o $@

CC3220S_LAUNCHXL.obj: ../../CC3220S_LAUNCHXL.c $(CONFIGPKG)/compiler.opt
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< -f $(CONFIGPKG)/compiler.opt -o $@

$(NAME).out: $(OBJECTS) $(CONFIGPKG)/linker.cmd
	@ echo linking...
	@ $(LNK)  $(OBJECTS) $(LFLAGS) -o $(NAME).out

clean:
	@ echo Cleaning...
	@ $(RM) $(OBJECTS) > $(DEVNULL) 2>&1
	@ $(RM) $(NAME).out > $(DEVNULL) 2>&1
	@ $(RM) $(NAME).map > $(DEVNULL) 2>&1
