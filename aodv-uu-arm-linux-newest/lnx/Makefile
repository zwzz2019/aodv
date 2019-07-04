obj-m += kaodv.o
kaodv-objs := kaodv-mod.o kaodv-debug.o kaodv-netlink.o kaodv-queue.o kaodv-ipenc.o kaodv-expl.o 

KOBJS := kaodv-mod.o kaodv-debug.o kaodv-netlink.o kaodv-queue.o kaodv-ipenc.o kaodv-expl.o
KOBJS_ARM := $(KOBJS:%.o=%-arm.o)

KERNEL=$(shell uname -r)
KERNEL_DIR=/lib/modules/$(KERNEL)/build
KERNEL_INC=$(KERNEL_DIR)/include
THIS_DIR=$(shell pwd)

.PHONY: clean indent default

#default: kaodv.ko
default: kaodv.ko
$(KOBJS): %.o: %.c Makefile
	$(CC) $(KCFLAGS) -c -o $@ $<

$(KOBJS_ARM): %-arm.o: %.c Makefile	
	$(KCC) $(KCFLAGS_ARM) -D ARM -c -o $@ $<

kaodv.o: #$(KOBJS) Makefile
	$(LD) -r $(KOBJS) -o $@
	cp $@ ../

kaodv-arm.o: $(KOBJS_ARM) Makefile
	$(LD) -r $(KOBJS_ARM) -o $@
	cp $@ ../kaodv.o

kaodv.ko: $(KOBJS:%.o=%.c) Makefile	
	$(MAKE) -C $(KERNEL_DIR) M=$(THIS_DIR) modules
	cp $@ ../

kaodv-arm.ko: $(KOBJS_ARM:%.o=%.c) Makefile
	$(MAKE) -C /lib/modules/4.4.30/build M=$(THIS_DIR) modules
	cp $@ ../kaodv.ko

clean:	
	$(MAKE) -C $(KERNEL_DIR) M=$(THIS_DIR) clean
	rm -rf *.o *~

TAGS:
	etags *.c *.h
indent:
	indent -kr -i8 -ts8 -sob -180 -ss -ncs *.c *.h
