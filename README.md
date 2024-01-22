# MI phones unlock bootloader using 2nd Xiaomi phone

## termux-miunlock setup and unlock
```shell
pkg update
pkg install git; pkg install vim
git clone https://github.com/MIUI-RUSSIAN/MIUnlock-termux.git . ; chmod +x setup.sh; chmod +x get_token.sh; ./setup.sh
mi-fastboot devices
mi-fastboot getvar product
mi-fastboot getvar token # Qualcomm Snapdragon CPU devices
mi-fastboot oem get_token # MTK CPU devices
./get_token.sh --product=PRODUCT --region=REGION --token=TOKEN DATA MI_ACCOUNT_DATA
mi-fastboot oem-unlock UNLOCK TOKEN
```

## Инструкция разблокировки bootloader-а с помощью второго устройства Xiaomi
Выполните построчно в termux код выше
