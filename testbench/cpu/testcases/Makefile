rwildcard=$(foreach d,$(wildcard $1*),$(call rwildcard,$d/,$2) $(filter $(subst *,%,$2),$d))
SRCS=$(call rwildcard, , *.s)

MEMS=$(SRCS:.s=.mem) 
ANSES=$(SRCS:.s=.ans)

info:
	@echo "Please use ./build.sh, do not make directly"

mem: $(MEMS) $(ANSES)

%.om: %.s
	$(ARCH)-as -mips32 -O0 -EB $< -o $@ 

%.ans: %.s
	sh genans.sh $< $@

%.mem: %.om
	$(ARCH)-objcopy --dump-section .text=/dev/stdout $< | xxd -c 4 -g 32 | cut -f 2 -d " " > $@

clean:
	rm *.mem *.ans || true
