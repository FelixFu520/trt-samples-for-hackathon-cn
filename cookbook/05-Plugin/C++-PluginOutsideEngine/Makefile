include ../../include/Makefile.inc

OBJ         = $(shell find . -name *.o 2>/dev/null)
DEP         = $(OBJ:.o=.d)

-include $(DEP)

all: main.exe AddScalarPlugin.so

main.exe: main.o cnpy.o
	$(NVCC) $(LDFLAG) -o $@ $+ -lz -lnvonnxparser

%.so: %.o
	$(NVCC) $(SOFLAG) $(LDFLAG) -o $@ $+

%.o: %.cpp
	$(NVCC) $(CUFLAG) $(INCLUDE) -M -MT $@ -o $(@:.o=.d) $<
	$(NVCC) $(CUFLAG) $(INCLUDE) -o $@ -c $<

%.o: %.cu
	$(NVCC) $(CUFLAG) $(INCLUDE) -M -MT $@ -o $(@:.o=.d) $<
	$(NVCC) $(CUFLAG) $(INCLUDE) -o $@ -c $<

.PHONY: test
test:
	make clean
	make -j
	python3 getOnnxModelAndData.py
	./main.exe
	./main.exe

.PHONY: clean
clean:
	rm -rf ./*.d ./*.o ./*.so ./*.exe ./*.plan

