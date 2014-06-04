CLEANFILES = \
	~* \
	*.txt \
	$(PACKAGE_NAME).axf \
	$(PACKAGE_NAME).as \
	$(PACKAGE_NAME).hex \
	$(PACKAGE_NAME).map \
	$(PACKAGE_NAME).bin

EXTRA_DIST = \
	./common

AUTOMAKE_OPTIONS = subdir-objects

#Add the directory dir to the list of directories to be searched for header files.
AM_CPPFLAGS =  -I $(top_builddir)/src/Source

LDSCRIPT = ./common/KERNEL/stm32_flash.ld

SOURCE = \
	./Source/main.c

HEADER = \
	$(LDSCRIPT)


bin_PROGRAMS = firmware-stm3210fx1
firmware_stm3210fx1_SOURCES = $(SOURCE) $(HEADER)
firmware_stm3210fx1_CFLAGS = $(uC_CFLAGS) $(LINKER_FLAGS) -Wa,-al=$(PACKAGE_NAME).as


# should use --gc-sections but the debugger does not seem to be able to cope with the option.
LINKER_FLAGS = -nostartfiles -Xlinker -o$(PACKAGE_NAME).axf -Xlinker -M -Xlinker -Map=$(PACKAGE_NAME).map -Xlinker --no-gc-sections

uC_CFLAGS = \
	-g \
	-Os \
	-std=gnu89 \
	-T $(LDSCRIPT) \
	-D STM32F10X_CL \
	-D USE_STM3210C_EVAL \
	-D VECT_TAB_FLASH \
	-D GCC_ARMCM3 \
	-D inline= \
	-D PACK_STRUCT_END=__attribute\(\(packed\)\) \
	-D ALIGN_STRUCT_END=__attribute\(\(aligned\(4\)\)\) \
	-mthumb \
	-mcpu=cortex-m3 \
	-ffunction-sections \
	-fdata-sections


log: $(PACKAGE_NAME).axf hex
	$(host_alias)-nm -n $(PACKAGE_NAME).axf > $(PACKAGE_NAME)_SymbolTable.txt
	$(host_alias)-size --format=SysV $(PACKAGE_NAME).axf > $(PACKAGE_NAME)_MemoryListingSummary.txt
	$(host_alias)-size $(firmware_stm3210fx1_OBJECTS) > $(PACKAGE_NAME)_MemoryListingDetails.txt

$(PACKAGE_NAME).axf: $(firmware_stm3210fx1_OBJECTS) Makefile
	$(CC) $(uC_CFLAGS) $(firmware_stm3210fx1_OBJECTS) $(LINKER_FLAGS)

hex : $(PACKAGE_NAME).axf
	$(host_alias)-objcopy $(PACKAGE_NAME).axf -O ihex $(PACKAGE_NAME).hex

