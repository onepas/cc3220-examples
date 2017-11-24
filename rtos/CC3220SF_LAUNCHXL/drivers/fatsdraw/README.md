## Example Summary

Example to read and write data onto a SD Card (SPI interface)

##Peripherals Exercised

* `Board_GPIO_LED0` - Indicates that the board was initialized within
`mainThread()`

* `Board_SDSPI0` - Connection to SD card

## Resources & Jumper Settings

> If you're using an IDE (such as CCS or IAR), please refer to Board.html in
your project directory for resources used and board-specific jumper settings.
Otherwise, you can find Board.html in the directory
&lt;SDK_INSTALL_DIR&gt;/source/ti/boards/&lt;BOARD&gt;.

This example requires an external SD Card boosterpack.

## Example Usage

* Example output is generated through use of Display driver APIs. Refer to the
Display driver documentation found in the SimpleLink MCU SDK User's Guide.

* Open a serial session (e.g. [`PuTTY`](http://www.putty.org/ "PuTTY's
Homepage"), etc.) to the appropriate COM port.
    *The COM port can be determined via Device Manager in Windows or via
`ls /dev/tty*` in Linux.

The connection will have the following settings:
```
    Baud-rate:     115200
    Data bits:          8
    Stop bits:          1
    Parity:          None
    Flow Control:    None
```

* Run the example. `Board_GPIO_LED0` turns ON to indicate driver initialization
is complete.

* The example proceeds to read the SD card. If an *input.txt* file
is not found, the file is created on the SD card with the following text:
```

    "***********************************************************************\n"
    "0         1         2         3         4         5         6         7\n"
    "01234567890123456789012345678901234567890123456789012345678901234567890\n"
    "This is some text to be inserted into the inputfile if there isn't     \n"
    "already an existing file located on the SDCard.                        \n"
    "If an inputfile already exists, or if the file was already once        \n"
    "generated, then the inputfile will NOT be modified.                    \n"
    "***********************************************************************\n"
```

* The *input.txt* file is then read and it's contents are written to a
new file called *output.txt*. If the file already exists on the SD card,
it will be overwritten.

* The contents of the *output.txt* file are then written to the console.

## Application Design Details

* The FatFs source is included and pre-built, together with support for
SYS/BIOS or FreeRTOS synchronization. The raw,
project-specific, API functions provided by FatFs (`f_open`, `f_write`) are used
within this example.

* This application demonstrates how to use the SD Card driver to read and
write data onto a SD Card using FatFS API calls (`f_open`, `f_read`, `f_write`,
etc).

* Users also have the option of writing to a RAM disk using either the CIO
runtime system calls or raw FatFS API's. See the initial setup below to
initialize the RAM disk prior to making any CIO or FatFS calls.

```
    #include <third_party/fatfs/ff.h>
    #include <third_party/fatfs/ffcio.h>
    #include <third_party/fatfs/diskio.h>

    unsigned int ramdiskData[RAMDISKSIZE];
    DRESULT result;

    result = ramdisk_start(ramdiskDriveId, ramdiskData, RAMDISKSIZE, 0);

    if (result != RES_OK) {
        /* ramdisk_start failed */
        while (1);
    }

```

> If using both the RAM disk and SD card filesystems, separate drive ID's must
be used.

This application uses one thread:

`mainThread` performs the following actions:

1. Creates and initializes SDSPI driver object.

2. Prints the disk size and the amount of free space on it.

3. Open the *input.txt* file. If not found, create the file for reading and
writing. Write the default message to the file.

4. Open the *output.txt* file. Content will be overwritten if found.

5. Copy the contents of *input.txt* to *output.txt*. Closes both files
afterwards.

6. Open *output.txt*, read the file, print the contents to
the display, and close *output.txt*.

7. Again, print the disk size and the amount of free space on it. If the
disk was empty, the difference in free space can be observed.

8. Close the SDSPI driver and terminate the example.

TI-RTOS:

* The `timespec` structure must be initialized with the current UNIX seconds
count in order to write files with accurate timestamps. This is due to the fact
that the BIOS Seconds module rather than the RTS library `time()` function
is used.

* When building in Code Composer Studio, the configuration project will be
imported along with the example. The configuration project is referenced by the
example, so it will be built first.  These projects can be found under
&lt;SDK_INSTALL_DIR&gt;/kernel/tirtos/builds/&lt;BOARD&gt;/(release|debug)/(ccs|gcc).


FreeRTOS:

* The compiler specific RTS library functions for `time()` and `localtime()`
are used. It is not necessary to initialize the `timespec` structure.

* Please view the `FreeRTOSConfig.h` header file for example configuration
information.

## References

* The FatFS libraries in this example are configured for use without Long File
Name (VFAT) or XFAT support. If you chose to reconfigure and build FatFS to
include these features, you may be required to license the appropriate patents
from Microsoft.
For more information, please refer to:
http://elm-chan.org/fsw/ff/en/appnote.html#lfn
    * The FatFs project homepage: http://elm-chan.org/fsw/ff/00index_e.html
