# Оптимизация GNOME

### 1. Отключение Tracker 3 в GNOME

```
systemctl --user mask tracker-miner-apps tracker-miner-fs tracker-store
```

После перезагрузки системы выполните чистим кэш tracker:

```
rm -rf ~/.cache/tracker ~/.local/share/tracker
```

### 2. Отключение ненужных GSD служб GNOME

GSD (gnome-settings-daemon) службы, это, как следует из названия, службы настройки GNOME и связанных приложений. Если отойти от строгого определения, то это просто службы-настройки на все случаи жизни, которые просто висят у вас в оперативной памяти в ожидании когда вам, или другому приложению, к примеру, понадобиться настроить/интегрировать поддержку планшета Wacom или других устройств. И другие подобные вещи.

#### 2.1. Отключение служб интеграции GNOME с графическим планшетом Wacom

Если у вас такого нет — смело отключайте:

```
systemctl --user mask org.gnome.SettingsDaemon.Wacom.service
```

#### 2.2. Отключение службы уведомления о печати&#x20;

Если нет принтера или вам просто не нужны эти постоянные уведомления — отключаем:

```
systemctl --user mask org.gnome.SettingsDaemon.PrintNotifications.service
```

#### 2.3. Отключение службы управления цветовыми профилями GNOME&#x20;

Отключив её, не будет работать тёплый режим экрана (Системный аналог Redshift):

```
systemctl --user mask org.gnome.SettingsDaemon.Color.service
```

#### 2.4. Отключение службы управления специальными возможностями системы&#x20;

**Не отключать людям с ограниченными возможностями!**

```
systemctl --user mask org.gnome.SettingsDaemon.A11ySettings.service
```

#### 2.5. Отключает службу управления беспроводными интернет-соединениями&#x20;

Не рекомендуется отключать для ноутбуков с активным использованием Wi-Fi:

```
systemctl --user mask org.gnome.SettingsDaemon.Wwan.service
```

#### 2.6. Отключение службы защиты от неавторизованных USB устройств при блокировке экрана

Можете оставить если у вас ноутбук:

```
systemctl --user mask org.gnome.SettingsDaemon.UsbProtection.service
```

#### 2.7. Отключаем службу настройки автоматической блокировки экрана&#x20;

Можете оставить если у вас ноутбук:

```
systemctl --user mask org.gnome.SettingsDaemon.ScreensaverProxy.service
```

#### 2.8. Отключение службы настройки общего доступа к файлам и директориям

```
systemctl --user mask org.gnome.SettingsDaemon.Sharing.service
```

#### 2.9. Отключение службы управления подсистемой rfkill

Отвечает за отключение любого радиопередатчика в системе (сюда относятся Wi-Fi и Bluetooth, поэтому данная служба нужна, скорее всего, для так называемого режима в "самолете"):

```
systemctl --user mask org.gnome.SettingsDaemon.Rfkill.service
```

#### 2.10. Отключение службы управления клавиатурой и раскладками GNOME&#x20;

Можно смело отключать если уже настроили все раскладки и настройки клавиатуры заранее, ибо все предыдущие настройки сохраняются при отключении:

```
systemctl --user mask org.gnome.SettingsDaemon.Keyboard.service
```

#### 2.11. Отключаем службу управления звуком GNOME&#x20;

Отключает **ТОЛЬКО** настройки звука GNOME, а не вообще всё управлением звуком в системе:

```
systemctl --user mask org.gnome.SettingsDaemon.Sound.service
```

#### 2.12. Отключение службы интеграции GNOME с карт-ридером

```
systemctl --user mask org.gnome.SettingsDaemon.Smartcard.service
```

#### 2.13. Отключение службы слежения за свободным пространством на диске&#x20;

Штука полезная, но если вы предпочитаете следить за этим самостоятельно, то вперед:

```
systemctl --user mask org.gnome.SettingsDaemon.Housekeeping.service
```

#### 2.14. Отключение службы управления питанием в GNOME&#x20;

Вы должны оставить эту службу включенной если у вас ноутбук, т. к. без неё не будет работать регулирование яркости:

```
systemctl --user mask org.gnome.SettingsDaemon.Power.service
```

#### 2.15. Отключение служб Evolution для синхронизации онлайн аккаунтов

```
systemctl --user mask evolution-addressbook-factory evolution-calendar-factory evolution-source-registry
```

Если после отключения какой-либо из вышеперечисленных служб что-то пошло не так, или просто какую-либо из них понадобилось снова включить, просто пропишите:

```
systemctl --user unmask --now СЛУЖБА
```

Служба вернется в строй после перезагрузки.

### 3. Ускорение загрузки системы (Отключение NetworkManager-wait-online)

Если вы пропишите команду _systemd-analyze blame_, то узнаете, что NetworkManager задерживает загрузку системы примерно на \~4 секунды. Чтобы это исправить выполните:

```
sudo systemctl mask NetworkManager-wait-online.service
```

### Результат

По окончании всех оптимизаций мы получаем потребление на уровне современной XFCE, но в отличие от оной уже на современном GTK4, а также со всеми рабочими эффектами и анимациями.

