CROSS_COMPILE?=arm-none-eabi-
CC=$(CROSS_COMPILE)gcc
LD=$(CROSS_COMPILE)ld
OBJCOPY=$(CROSS_COMPILE)objcopy
OBJDUMP=$(CROSS_COMPILE)objdump
OPENOCD?=openocd

LD_SCRIPT=blinky.lds
CFLAGS=-mcpu=cortex-m3 -mthumb

default: blinky.hex blinky.dis

program: blinky.hex
	$(OPENOCD) -f ../nrf52.cfg -c "halt ; nrf51 mass_erase ; program blinky.hex verify reset ; exit"

%.dis: %.elf
	$(OBJDUMP) -D $< > $@

%.hex: %.elf
	$(OBJCOPY) $< $@ -O ihex

%.elf: %.o
	$(LD) -T $(LD_SCRIPT) $< -o $@

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f blinky.o blinky.elf blinky.hex blinky.dis
