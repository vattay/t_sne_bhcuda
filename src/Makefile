BIN               := t_sne_bhcuda
CXXFLAGS          := -O3 -g -std=c++11


CUDA_INSTALL_PATH ?= /usr/local/cuda

NVCC ?= $(CUDA_INSTALL_PATH)/bin/nvcc
INCD = -I"$(CUDA_INSTALL_PATH)/include" -I"./"
LIBS = -lcuda -L"$(CUDA_INSTALL_PATH)/lib64" -lcudart -lcublas -lcufft -lstdc++ -lpthread
NVCCFLAGS := -m64 --ptxas-options=-v -O3 -G -g -arch=sm_61


# files
CPP_SOURCES       := sptree.cpp t_sne_gpu.cpp
CU_SOURCES        := check_cuda_device.cu kernels.cu
HEADERS           := $(wildcard *.h)
CPP_OBJS          := $(patsubst %.cpp, %.o, $(CPP_SOURCES))
CU_OBJS           := $(patsubst %.cu, %.cu_o, $(CU_SOURCES))

%.cu_o : %.cu
	$(NVCC) $(NVCCFLAGS) -c $(INCD) -o $@ $<

%.o: %.cpp
	$(CXX) -c $(CXXFLAGS) $(INCD) -o $@ $<

$(BIN): $(CPP_OBJS) $(CU_OBJS)
	$(CXX) -o $(BIN) $(CU_OBJS) $(CPP_OBJS) $(LDFLAGS) $(INCD) $(LIBS)

clean:
	rm -f $(BIN) *.o *.cu_o

