# GPU Passthrough
## 修改 /etc/default/grub
```bash
nano /etc/default/grub
```

找到以下的字串:
```
GRUB_CMDLINE_LINUX_DEFAULT=”quiet”
```

然後改成以下的字串:
若是Intel CPU則修改如下:
```
GRUB_CMDLINE_LINUX_DEFAULT=”quiet intel_iommu=on”
```

若是 AMD CPU則修改如下:
```
GRUB_CMDLINE_LINUX_DEFAULT=”quiet amd_iommu=on”
```

特定Xeon處理器：
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt pcie_acs_override=downstream,multifunction nofb nomodeset video=vesafb:off,efifb:off"
```
更新 grub
```
update-grub
```

## 修改/etc/modules
```
nano /etc/modules
```

輸入
```
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```

## 修改IOMMU
```
echo "options vfio_iommu_type1 allow_unsafe_interrupts=1" > /etc/modprobe.d/iommu_unsafe_interrupts.conf
echo "options kvm ignore_msrs=1" > /etc/modprobe.d/kvm.conf
```

## 將不必要的驅動程式設成給黑名單
```
echo "blacklist radeon" >> /etc/modprobe.d/blacklist.conf
echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf
echo "blacklist nvidia" >> /etc/modprobe.d/blacklist.conf
```

## 將GPU指派給vfio
```
lspci -v
```
得到如
```
04:00.0 3D controller: NVIDIA Corporation TU104GL [Tesla T4] (rev a1)
        Subsystem: NVIDIA Corporation Device 12a2
        Flags: bus master, fast devsel, latency 0, IRQ 129, NUMA node 0, IOMMU group 27
        Memory at 91000000 (32-bit, non-prefetchable) [size=16M]
        Memory at 3bfe0000000 (64-bit, prefetchable) [size=256M]
        Memory at 3bff0000000 (64-bit, prefetchable) [size=32M]
        Capabilities: [60] Power Management version 3
        Capabilities: [68] MSI: Enable+ Count=1/1 Maskable- 64bit+
        Capabilities: [78] Express Endpoint, IntMsgNum 0
        Capabilities: [100] Virtual Channel
        Capabilities: [250] Latency Tolerance Reporting
        Capabilities: [258] L1 PM Substates
        Capabilities: [128] Power Budgeting <?>
        Capabilities: [420] Advanced Error Reporting
        Capabilities: [600] Vendor Specific Information: ID=0001 Rev=1 Len=024 <?>
        Capabilities: [900] Secondary PCI Express
        Capabilities: [bb0] Physical Resizable BAR
        Kernel driver in use: nouveau
        Kernel modules: nvidiafb, nouveau
```

```
lspci -n -s 04:00
```
得到
```
04:00.0 0302: 10de:1eb8 (rev a1)
```


```
echo "options vfio-pci ids=10de:1eb8 disable_vga=1" > /etc/modprobe.d/vfio.conf
```

```
update-initramfs -u
```
