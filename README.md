# CentOSWSL
 Install CentOS as a WSL Instance (for Windows 10 FCU 64bit or later)

Установка CentOS по мотивам

https://docs.microsoft.com/en-us/windows/wsl/install-on-server

https://medium.com/@gauravnelson/how-to-run-fedora-with-windows-subsystem-for-linux-wsl-a148902e5087

https://github.com/RoliSoft/WSL-Distribution-Switcher

https://github.com/yuk7/ArchWSL

Начнем с создания виртуального диска

https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/boot-to-vhd--native-boot--add-a-virtual-hard-disk-to-the-boot-menu

`diskpart`

особое внимание обращаем на права доступа, SD="D:P(A;;GA;;;WD)" , без них будет ошибка, доступ ПОЛНЫЙ для ВСЕх

```
create vdisk file=J:\windows.vhdx maximum=100000 type=expandable SD="D:P(A;;GA;;;WD)"
select vdisk file=J:\windows.vhdx
attach vdisk
create partition primary active
format fs=ntfs quick label="WSL"
assign letter=F
```

`exit`

```
cd f:
f:
```

https://github.com/CentOS/sig-cloud-instance-images/tree/CentOS-7.5.1804/docker

`wget https://github.com/CentOS/sig-cloud-instance-images/blob/CentOS-7.5.1804/docker/centos-7-docker.tar.xz?raw=true`
