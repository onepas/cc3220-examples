SIMPLELINK_CC32XX_SDK_INSTALL_DIR ?= $(abspath ../../../../../..)

include $(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/imports.mak

CC = "$(CCS_ARMCOMPILER)/bin/armcl"
LNK = "$(CCS_ARMCOMPILER)/bin/armcl"

OBJECTS = uart_term.obj main_nortos.obj mqueue_nortos.obj cloud_ota.obj platform.obj CC3220SF_LAUNCHXL.obj

NAME = cloud_ota

CFLAGS = -I.. -DCC32XX "-I$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source" -DNORTOS_SUPPORT "-I$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/kernel/nortos" "-I$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/kernel/nortos/posix" --silicon_version=7M4 --code_state=16 --little_endian --display_error_number --diag_warning=255 --diag_wrap=off --gen_func_subsections=on --float_support=vfplib --symdebug:dwarf "-I$(CCS_ARMCOMPILER)/include"

LFLAGS = "-l$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/net/ota/ccs/Release/ota.a" "-l$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/net/json/ccs/Release/json.a" "-l$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/display/lib/display.aem4" "-l$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/drivers/lib/drivers_cc32xx.aem4" "-l$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/third_party/fatfs/lib/fatfs.aem4" --diag_suppress=10063 --entry_point=resetISR "-l$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/drivers/net/wifi/ccs/nortos/simplelink.a" "-l$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/kernel/nortos/lib/nortos_cc32xx.aem4" "-l$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/devices/cc32xx/driverlib/ccs/Release/driverlib.a" ../ccs/CC3220SF_LAUNCHXL_NoRTOS.cmd "-m$(NAME).map" --warn_sections --display_error_number --diag_wrap=off --rom_model "-i$(CCS_ARMCOMPILER)/lib" -llibc.a

all: $(NAME).out

uart_term.obj: ../uart_term.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< --output_file=$@

main_nortos.obj: ../main_nortos.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< --output_file=$@

mqueue_nortos.obj: ../mqueue_nortos.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< --output_file=$@

cloud_ota.obj: ../cloud_ota.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< --output_file=$@

platform.obj: ../platform.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< --output_file=$@

CC3220SF_LAUNCHXL.obj: ../CC3220SF_LAUNCHXL.c
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< --output_file=$@

$(NAME).out: $(OBJECTS)
	@ echo linking...
	@ $(LNK) $(CFLAGS) -z $(OBJECTS) $(LFLAGS) -o $(NAME).out

clean:
	@ echo Cleaning...
	@ $(RM) $(OBJECTS) > $(DEVNULL) 2>&1
	@ $(RM) $(NAME).out > $(DEVNULL) 2>&1
	@ $(RM) $(NAME).map > $(DEVNULL) 2>&1
