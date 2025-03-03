TARGET := vos.elf

CC = cc
V = v

CFLAGS = -O3 -pipe

LDINTERNALFLAGS :=  \
	-Tlinker.ld \
	-nostdlib   \
	-Wl,-gc-sections \
	-shared     \
	-pie -fno-pic -fpie \
	-z max-page-size=0x1000

INTERNALCFLAGS  :=       \
	-I.                  \
	-ffreestanding       \
	-fno-stack-protector \
	-fno-pic -fpie       \
	-ffunction-sections  \
	-fdata-sections      \
	-mno-red-zone

INTERNALVFLAGS := \
	-shared \
	--enable-globals

VFILES := $(shell find ./ -type f -name '*.v')
CFILES := $(shell find ./ -type f -name '*.c')
OBJ    := $(CFILES:.c=.o) blob.o

.PHONY: all clean

all: $(TARGET)

$(TARGET): $(OBJ)
	$(CC) $(LDINTERNALFLAGS) $(OBJ) -o $@

blob.o: blob.C
	$(CC) -x c $(CFLAGS) $(INTERNALCFLAGS) -c $< -o $@

%.o: %.c
	$(CC) -Wall -Wextra $(CFLAGS) $(INTERNALCFLAGS) -c $< -o $@

blob.C: $(VFILES)
	$(V) $(VFLAGS) $(INTERNALVFLAGS) -o $(@:.C=.c) .
	mv $(@:.C=.c) $@

clean:
	rm -rf $(TARGET) $(OBJ) blob.C
