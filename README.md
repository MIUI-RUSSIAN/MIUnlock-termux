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

## Инструкция разблокировки bootloader-а с помощью второго устройства Xiaomi и терминала Termux

Программа, с помощью которой можно получить токен разблокировки загрузчика для устройств Xiaomi. (и разблокировать загрузчик) с помощью Termux.

> Примечание: Этот инструмент не может обойти 7, 14, 30-дневный срок разблокировки.

### Использование

```
Usage: get_token.sh [OPTIONS] DATA
A program that can be used to retrieve the bootloader unlock token for Xiaomi
devices. using Termux.
*     DATA                Install account.apk from repo, login and copy-paste
                            the response.
      --debug             Output messages about what the tool is doing
      --help              Display a help message
*     --product=PRODUCT   Used to verify device product
      --region=REGION     Tool server hosts or regions: india, global, china,
                            russia, europe
                            Default: india
*     --token=TOKEN       Used to verify device token
      --version           Version information
```

### Требования

1. Верифицированная учетная запись Xiaomi
2. Два устройства Android (принимающее и целевое)
3. Кабель USB Otg & Data
4. Подключение к Интернету

### Подробная инструкция

1. Установите необходимые приложения [termux](https://github.com/termux/termux-app), [termux-api](https://github.com/termux/termux-api) и `account.apk` на ваше устройство.
2. Войдите и привяжите свой аккаунт xiaomi на целевом устройстве.
3. Клонируйте это репо.
   ```shell
   git clone git clone https://github.com/MIUI-RUSSIAN/MIUnlock-termux.git && cd termux-miunlock
   ```

4. Запустите `setup.sh` для установки необходимых пакетов.
   ```shell
   chmod +x setup.sh && ./setup.sh
   ```

5. Получите `product` устройства
   ```shell
   mi-fastboot getvar product
   ```

6. Получите `token` устройства **Snapdragon**
   ```shell
   mi-fastboot getvar token
   ```

7. Получите `token` устройства **MTK**
   ```shell
   mi-fastboot oem get_token
   ```
   Если вы получили 2 или 3 токена, объедините их, пример:
   ```shell
   # Перед
   (bootloader) token: VQECMAEQTSdjm281zqPylolzfxy3bQMGbWVy
   (bootloader) token: bGluAhTVfQBXJGUJ78qoZQ0ctBDLQ1PkJg==
   # После
   VQECMAEQTSdjm281zqPylolzfxy3bQMGbWVybGluAhTVfQBXJGUJ78qoZQ0ctBDLQ1PkJg==
   ```
8. Запустите скрипт ``get_token.sh`` с необходимыми аргументами
   ```shell
   chmod +x get_token.sh
   ./get_token.sh --region=global --product=PRODUCT --token=TOKEN DATA
   ```
   Если код работает успешно, он выдаст вам длинную строку, которая и будет являться токеном разблокировки
   Вы должны передать правильный регион, который вы использовали в account.apk, если вы получили ошибку 20045
   доступные варианты: `global, russia, china, europe, indiaб taiwan` (`глобалка, Россия, Китай, европа, Индия, Тайвань`)
   ```shell
   ./get_token.sh --region=REGION --product=PRODUCT --token=TOKEN DATA
   ```
9. Преобразуйте строку токена разблокировки в двоичный токен
   ```shell
   echo "UNLOCK_TOKEN" | xxd -r -p > token.bin
   ```
10. Введите:
    ```shell
    mi-fastboot stage token.bin && mi-fastboot oem unlock
    ```
    Или (пропустите шаг 9):
    ```shell
    mi-fastboot oem-unlock "UNLOCK_TOKEN"
    ```
11. Устройство успешно сбросит заводские настройки и разблокируется.

### Источники:

* https://www.youtube.com/watch?v=RNqW734X1vw (Осторожно, Индийский английский)
* https://github.com/Findra2006/termux-miunlock
* https://github.com/RohitVerma882/termux-miunlock

### Проверка

Будет выполняться на устройствах Mi MIX3 (rooted) и Xiaomi 13 Pro как только я сделаю резервную копию на 13м с HyperOS 1.0.2.0 (TW)

### Автор

Переведено: Xenon007
Специально для: MIUI.RU, HyperOS.News, 4pda

#### C ❤️ к MIUI/HOS
