# nvidia GPU

* nvidia-smiなしでGPU及び推奨ドライバ確認
  ```
  # ubuntu-drivers devices
  == /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0 ==
  modalias : pci:v000010DEd00001BB1sv0000103Csd000011A3bc03sc00i00
  vendor   : NVIDIA Corporation
  model    : GP104GL [Quadro P4000]
  driver   : nvidia-driver-390 - third-party free
  driver   : nvidia-driver-435 - distro non-free
  driver   : nvidia-driver-440 - third-party free
  driver   : nvidia-driver-415 - third-party free
  driver   : nvidia-driver-410 - third-party free
  driver   : nvidia-driver-470 - third-party free recommended
  driver   : xserver-xorg-video-nouveau - distro free builtin
  ```