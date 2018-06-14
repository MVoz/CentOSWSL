# CentOSWSL
 Install CentOS as a WSL Instance (for Windows 10 FCU 64bit or later)

Установка CentOS по мотивам

https://docs.microsoft.com/en-us/windows/wsl/install-on-server

https://medium.com/@gauravnelson/how-to-run-fedora-with-windows-subsystem-for-linux-wsl-a148902e5087

https://github.com/DDoSolitary/LxRunOffline

https://github.com/RoliSoft/WSL-Distribution-Switcher

ArchWSL

https://github.com/yuk7/ArchWSL

Начнем с создания виртуального диска

https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/boot-to-vhd--native-boot--add-a-virtual-hard-disk-to-the-boot-menu

```
cmd.exe
diskpart
```

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


```
bash.exe
wget https://github.com/CentOS/sig-cloud-instance-images/blob/CentOS-7.5.1804/docker/centos-7-docker.tar.xz?raw=true`
```

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


п.с. если в терминале выходит ошибка, то можно попробывать сделать так

https://github.com/Microsoft/WSL/issues/3020#issuecomment-389662966

только не удалить, а наоборот создать, у меня по крайней мере этого файла небыло, C:\Windows\System32\CodeIntegrity\SIPolicy.p7b, создать пустышку, главное чтоб имя было SIPolicy.p7b

некотрые интересные параметры находятся здесь, HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Lxss

допустим чтобы WSL не брал переменные с Виндовс, а это заложено по умолчанию, можно добавить

```
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Lxss]
"AppendNtPath"=dword:00000000
```

ветка с CentOS

```
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Lxss\{8fa2e5cc-b32b-4e49-8e17-333d5e353907}]
"DistributionName"="CentOS"
"State"=dword:00000001
"Version"=dword:00000001
"BasePath"="F:\\CentOS"
"DefaultEnvironment"=hex(7):48,00,4f,00,53,00,54,00,54,00,59,00,50,00,45,00,3d,\
  00,78,00,38,00,36,00,5f,00,36,00,34,00,00,00,4c,00,41,00,4e,00,47,00,3d,00,\
  72,00,75,00,5f,00,52,00,55,00,2e,00,55,00,54,00,46,00,2d,00,38,00,00,00,50,\
  00,41,00,54,00,48,00,3d,00,2f,00,75,00,73,00,72,00,2f,00,6c,00,6f,00,63,00,\
  61,00,6c,00,2f,00,73,00,62,00,69,00,6e,00,3a,00,2f,00,75,00,73,00,72,00,2f,\
  00,6c,00,6f,00,63,00,61,00,6c,00,2f,00,62,00,69,00,6e,00,3a,00,2f,00,75,00,\
  73,00,72,00,2f,00,73,00,62,00,69,00,6e,00,3a,00,2f,00,75,00,73,00,72,00,2f,\
  00,62,00,69,00,6e,00,3a,00,2f,00,73,00,62,00,69,00,6e,00,3a,00,2f,00,62,00,\
  69,00,6e,00,3a,00,2f,00,75,00,73,00,72,00,2f,00,67,00,61,00,6d,00,65,00,73,\
  00,3a,00,2f,00,75,00,73,00,72,00,2f,00,6c,00,6f,00,63,00,61,00,6c,00,2f,00,\
  67,00,61,00,6d,00,65,00,73,00,00,00,54,00,45,00,52,00,4d,00,3d,00,78,00,74,\
  00,65,00,72,00,6d,00,2d,00,32,00,35,00,36,00,63,00,6f,00,6c,00,6f,00,72,00,\
  00,00,00,00
"DefaultUid"=dword:00000000
"DistributionFlags"=dword:00000007
"Flags"=dword:00000007
"KernelCommandLine"="BOOT_IMAGE=/kernel init=/init rw"
```


`DefaultEnvironment = `
```
HOSTTYPE=x86_64
LANG=ru_RU.UTF-8
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games
TERM=xterm-256color
```
