# SPDX-License-Identifier: GPL-2.0

# Define KERNEL_SRC to locate the kernel source for DKMS builds
KERNEL_SRC ?= /lib/modules/$(shell uname -r)/build
obj-m += rknpu.o

# Include directory for headers
ccflags-y += -I$(srctree)/$(src)/include
ccflags-y += -I$(src)/include
# ccflags-y += -Werror

# Object files for the rknpu module
rknpu-y += rknpu_drv.o
rknpu-y += rknpu_reset.o
rknpu-y += rknpu_job.o
rknpu-y += rknpu_debugger.o
rknpu-y += rknpu_iommu.o
rknpu-$(CONFIG_PM_DEVFREQ) += rknpu_devfreq.o
rknpu-$(CONFIG_ROCKCHIP_RKNPU_SRAM) += rknpu_mm.o
rknpu-$(CONFIG_ROCKCHIP_RKNPU_FENCE) += rknpu_fence.o
rknpu-$(CONFIG_ROCKCHIP_RKNPU_DRM_GEM) += rknpu_gem.o
rknpu-$(CONFIG_ROCKCHIP_RKNPU_DMA_HEAP) += rknpu_mem.o

# For non-Rockchip kernels: force-enable DRM GEM path when DRM is available
ifneq ($(CONFIG_DRM),)
ifndef CONFIG_ROCKCHIP_RKNPU_DRM_GEM
ccflags-y += -DCONFIG_ROCKCHIP_RKNPU_DRM_GEM
rknpu-y += rknpu_gem.o
endif
endif

# For non-Rockchip kernels: force-enable fence when SYNC_FILE is available
ifneq ($(CONFIG_SYNC_FILE),)
ifndef CONFIG_ROCKCHIP_RKNPU_FENCE
ccflags-y += -DCONFIG_ROCKCHIP_RKNPU_FENCE
rknpu-y += rknpu_fence.o
endif
endif

# Build and clean targets for DKMS compatibility
all:
	$(MAKE) -C $(KERNEL_SRC) M=$(PWD) modules

clean:
	$(MAKE) -C $(KERNEL_SRC) M=$(PWD) clean

