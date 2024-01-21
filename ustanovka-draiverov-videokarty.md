---
description: >-
  У автора нет видеокарты от AMD, поэтому будет описана только установка
  драйверов Nvidia.
---

# Установка драйверов видеокарты

## Установка драйверов NVIDIA с помощью RPM Fusion

Многие пользователи предпочитают использовать репозиторий RPM Fusion для установки драйверов NVIDIA, поскольку это более простой способ. Кроме того, возможно, он не предлагает самые последние драйверы, но он точно предлагает последние драйверы, которые протестированы и поддерживаются сообществом Fedora.

Однако если вы используете репозиторий RPM Fusion для установки драйверов NVIDIA, то они будут автоматически получать обновления вместе с вашей системой.

### **1. Установка заголовков ядра и средств разработки**

```
sudo dnf install kernel-devel kernel-headers gcc make dkms acpid libglvnd-glx libglvnd-opengl libglvnd-devel pkgconfig
```

### **2. Установка драйвера NVIDIA и поддержка CUDA**

```
sudo dnf install akmod-nvidia xorg-x11-drv-nvidia xorg-x11-drv-nvidia-libs xorg-x11-drv-nvidia-cuda xorg-x11-drv-nvidia-power nvidia-settings
```

Если используется 64-битная ОС, но требуется запускать ещё и Steam и 32-битные версии игр, установим также 32-битный драйвер

```
sudo dnf install xorg-x11-drv-nvidia-libs.i686
```

Подождём 3-5 минут и убедимся, что модули были успешно собраны:

```
sudo akmods --force
```

### 3. Пересоберём образ initrd:

```
sudo dracut --force
```

### 4. Активируем systemd-юниты для корректной работы спящего режима и гибернации:

```
sudo systemctl enable nvidia-{suspend,resume,hibernate}
```

### **5.** Отключение драйверов Nouveau

```
echo -e "blacklist nouveaunoptions nouveau modeset=0" | sudo tee /etc/modprobe.d/blacklist-nouveau.conf
```

### 6. Включите nvidia-modeset

Пригодится, если у вас есть ноутбук с графическим процессором Nvidia. Необходим для некоторых функций взаимодействия, связанных с PRIME:

```
sudo grubby --update-kernel=ALL --args="nvidia-drm.modeset=1"
```

### 7. Изменение конфигурации GDM

Введите команду `sudo nano /etc/gdm/custom.conf` и замените `#WaylandEnable=false` на `WaylandEnable=true`.

