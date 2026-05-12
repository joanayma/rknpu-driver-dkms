# rknpu-driver-dkms

DKMS kernel driver for Rockchip NPUs (RK3566, RK3568, RK3588, RV1106, RK3562, RK3576).

Tested on Fedora ARM Rawhide (aarch64) with kernel 6.14+.

## Supported SoCs

| SoC | Compatible string |
|-----|-------------------|
| RK3566 | `rockchip,rk3566-rknpu` |
| RK3568 | `rockchip,rk3568-rknpu` |
| RK3588 | `rockchip,rk3588-rknpu` |
| RV1106 | `rockchip,rv1106-rknpu` |
| RK3562 | `rockchip,rk3562-rknpu` |
| RK3576 | `rockchip,rk3576-rknpu` |
| Generic | `rockchip,rknpu` |

## Build & Install

### Via DKMS (recommended)

```sh
sudo dkms add rknpu-0.9.8
sudo dkms build rknpu/0.9.8
sudo dkms install rknpu/0.9.8
sudo modprobe rknpu
```

### Manual build

```sh
cd rknpu-0.9.8
make -C /lib/modules/$(uname -r)/build M=$(pwd) modules
sudo make -C /lib/modules/$(uname -r)/build M=$(pwd) modules_install
sudo depmod
sudo modprobe rknpu
```

### Verify

```sh
lsmod | grep rknpu
dmesg | grep rknpu
```

## Origin

Based on GPL-2.0 `rknpu-0.9.8` driver from [airockchip/rknn-llm](https://github.com/airockchip/rknn-llm).

Local copies of Rockchip headers (`rockchip_opp_select.h`, `rockchip_system_monitor.h`, `rockchip_ipa.h`) are included for compatibility with non-Rockchip kernel trees.
