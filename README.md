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

дальше подразумевается что уже стоит Ubuntu либо WSL либо в другой системе, в виртуалке или на живой системе

заходим в убунту, bash.exe или по другому, кто как привык

скачиваем дистриб для докера

https://github.com/CentOS/sig-cloud-instance-images/tree/CentOS-7.5.1804/docker

`wget https://github.com/CentOS/sig-cloud-instance-images/blob/CentOS-7.5.1804/docker/centos-7-docker.tar.xz?raw=true`

распаковываем его в папку tmp

```
mkdir tmp
tar -xf centos-7-docker.tar.xz -C tmp
cd tmp
```

собираем заного с исправлениями, параметр `--ignore-case` обязателен, иначе будут ошибки

`tar -czf ../centos.tar.gz --ignore-case --hard-dereference *`

выходим из убунты, баш, закрываем терминал

скачиваем установщик WSL OS, для удобства

```
wget https://github.com/DDoSolitary/LxRunOffline/releases/download/v2.2.2/LxRunOffline-v2.2.2.zip
mkdir LxRunOffline
tar -xf LxRunOffline-v2.2.2.zip -C LxRunOffline
```

Установим CentOS

`F:\LxRunOffline\LXRunOffline.exe install -n CentOS -f F:\centos.tar.gz -d F:\CentOS`

Зайдем в БАШ по умолчанию под рутом, другого же пользователя мы еще не сделали

`F:\LxRunOffline\LxRunOffline run -n CentOS -c bash`

`yum update`

ну и т.д.
