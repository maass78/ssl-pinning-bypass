## FRIDA

##### Ссылки: 
- [Ебейший скрипт (использовать его)](https://github.com/httptoolkit/frida-interception-and-unpinning)
- [Тоже вроде неплохой, работает, еще есть защита от обнаружения рута, но это не проверял](https://github.com/themalwarenews/frida_rootandsslbypass/blob/main/rootandsslbypass.js)


##### Подготовка:
- Включить рут на ноксе

- Установить фриду (нужен питон)
    ```
    pip install Frida
    pip install objection
    pip install frida-tools
    ```

- Перейти в директорию с адб и сервером фриды (platform-tools.zip распаковать)
`cd C:\platform-tools`

- Законектиться по адб, порт может быть `62025`
`adb connect 127.0.0.1:62001`

- Скопировать фриду 
`adb push frida-server /data/local/tmp`

- Если выдает что память read-only прописать (перезагружать не надо)
    ```
    adb remount
    adb shell
    mount -o rw,remount /
    exit
    ```

- Потом `adb shell chmod 777 /data/local/tmp/frida-server`

- Запускаем фриду на девайсе `adb shell /data/local/tmp/frida-server &`


##### Во второй cmd'шке инжектим в приложение скрипт с помощью фриды:

- Узнаем packageName с помощью `frida-ps -U ` либо `frida-ps -Ua ` либо `frida-ps -Uai`

- Запускаем приложение `frida -U -l scriptname.js -f com.package.name`
 
> Конкретно с [frida-interception-and-unpinning](https://github.com/httptoolkit/frida-interception-and-unpinning): `frida -U -l config.js -l android-proxy-override.js -l android-system-certificate-injection.js -l android-certificate-unpinning.js -l android-certificate-unpinning-fallback.js -f com.package.name`

##### Кратко:

```
cd C:\platform-tools
adb connect 127.0.0.1:62001 / 62025

adb remount
adb shell
mount -o rw,remount /
exit

adb push frida-server /data/local/tmp 
adb shell chmod 777 /data/local/tmp/frida-server
adb push fridascript.js /data/local/tmp
adb shell /data/local/tmp/frida-server &

frida-ps -U
frida -U -l scriptname.js -f com.package.name
```

## magisk + lsposed
[Туториал на ютубе](https://youtu.be/Yox5UOc7Gms?si=FqK1KR9vT_bloY9p)
- Создаем эмулятор с **android 9** на седьмом нихуя не заведется 
- Устанавливаем magisk, жмем инсталл, ставим галочку на что-то связанное с адб, потом выбираем патчить boot, открывается проводник, его и magisk закрываем, заходим снова в magisk, кликаем в последний из трех радиобатонов. После установки не перезагружаемся сразу, отключаем рут, потом закрываем и открываем nox заново
- Включаем Zygisk в настройках magisk'а
- Ребутаемся
- Устанавливаем lsposed в magisk
- Радуемся жизни (нет)
