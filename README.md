## `dxgkrnl`, DirectX Support Driver for WSL2

Author: Microsoft Corporation

- [x] Modifications to the original code were made to enable the function on newer kernel revisions

---

[Read information here](https://devblogs.microsoft.com/directx/directx-heart-linux/)

Dxgkrnl is a brand-new kernel driver for Linux that exposes the /dev/dxg device to user mode Linux. /dev/dxg exposes a set of IOCTL that closely mimic the native WDDM D3DKMT kernel service layer on Windows. Dxgkrnl inside of the Linux kernel connects over the VM Bus to its big brother on the Windows host and uses this VM bus connection to communicate with the physical GPU.

![dxgkrnl working mechanism](https://devblogs.microsoft.com/directx/wp-content/uploads/sites/42/2020/05/word-image-7.png)

If the host has multiple GPUs, all GPUs are projected and available to the Linux environment (assuming all of these GPUs are running WDDMv2.9 drivers).

Applications running inside of the Linux environment have the same access to the GPU as native applications on Windows. There is no partitioning of resources between Linux and Windows or limit imposed on Linux applications. The sharing is completely dynamic based on who needs what. There are basically no differences between two Windows applications sharing a GPU versus a Linux and a Windows application sharing the same GPU. If a Linux application is alone on a GPU, it can consume all its resources!

Assuming you have the right GPU driver installed on the Windows host, /dev/dxg is automatically exposed and available to any WSL distro installed without having to install any additional packages. Note that the distro needs to be running in WSL version 2 mode (wsl –set-version \<Distro\> 2) in order to get access to the GPU.

Although they share a name, the version of dxgkrnl inside of the Linux kernel is a clean room implementation of a Linux GPU driver based on our GPU-PV protocol and doesn’t share anything else in common with its similarly named Windows counterpart. Dxgkrnl Linux edition is being made open source and shared back with the community. As we work on upstreaming this new driver, source code is available in Microsoft’s official Linux kernel branch for WSL 2.

[https://github.com/microsoft/WSL2-Linux-Kernel/tree/linux-msft-wsl-4.19.y/drivers/gpu/dxgkrnl](https://github.com/microsoft/WSL2-Linux-Kernel/tree/linux-msft-wsl-4.19.y/drivers/gpu/dxgkrnl)
