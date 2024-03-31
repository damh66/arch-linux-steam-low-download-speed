# Исправление низкой скорости загрузки в Steam на Arch Linux

Устанавливаем **`dnsmasq`**:
```
sudo pacman -S dnsmasq
```

Делаем резервную копию конфигурационного файла **`dnsmasq`**:
```
cp /etc/dnsmasq.conf /etc/dnsmasq.conf_old
```

Приводим файл **`/etc/dnsmasq.conf`** к следующему виду:
```
#адрес, который будет слушать dnsmasq
listen-address=127.0.0.1

#запрещаем чтение файла /etc/resolv.conf для получения dns-серверов
no-resolv

#порт, на котором будет работать dnsmasq
port=0

#dns-сервера
server=8.8.8.8
server=8.8.4.4
```

Редактируем **`/etc/NetworkManager/NetworkManager.conf`**:
```
[main]
dns=dnsmasq
```

Перезапускаем **`NetworkManager`** и **`dnsmasq`**:
```
sudo systemctl restart NetworkManager
sudo systemctl restart dnsmasq
```

В файле **`~/.steam/steam/steam_dev.cfg`**(создаем, если его нет) прописываем:
```
@nClientDownloadEnableHTTP2PlatformLinux 0
@fDownloadRateImprovementToAddAnotherConnection 1.0
```
> Если вы используете **Flatpak**, файл должен находиться по пути:
> ```
>  ~/.var/app/com.valvesoftware.Steam/.steam/steam/steam_dev.cfg
> ```

**После перезапуска Steam скорость загрузки должна вернуться в норму**

<p align="center">
  <img src="https://github.com/damh66/arch-linux-steam-low-download-speed/assets/109068342/4429fc6a-85d0-43dc-bcf4-96f6e1f9a5cc" align="middle"/>

</p>
