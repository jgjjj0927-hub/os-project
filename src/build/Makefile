# language: Makefile, file: src/build/Makefile, target: os-project i386
# Сборка: os-image.bin (bootsect + kernel), ISO отдельно
CC      = i386-elf-gcc
LD      = i386-elf-ld
NASM    = nasm
CFLAGS  = -g -ffreestanding -c

OBJS = kernel_entry.o kernel.o utils.o lowlevel_io.o screen.o common.o

.PHONY: all clean run

all: os-image.bin

os-image.bin: bootsect.bin kernel.bin
	cat bootsect.bin kernel.bin > os-image.bin

bootsect.bin:
	cd ../boot && $(NASM) bootsect.asm -f bin -o ../build/bootsect.bin

kernel.bin: $(OBJS)
	$(LD) -o $@ -Ttext 0x1000 $^ --oformat binary

kernel_entry.o: ../boot/kernel_entry.asm
	$(NASM) $< -f elf -o $@

kernel.o: ../kernel/kernel.c
	$(CC) $(CFLAGS) $< -o $@

utils.o: ../kernel/utils.c
	$(CC) $(CFLAGS) $< -o $@

lowlevel_io.o: ../drivers/lowlevel_io.c
	$(CC) $(CFLAGS) $< -o $@

screen.o: ../drivers/screen.c
	$(CC) $(CFLAGS) $< -o $@

common.o: ../common.c
	$(CC) $(CFLAGS) $< -o $@

run: os-image.bin
	qemu-system-i386 -fda os-image.bin

clean:
	rm -f *.o *.bin
