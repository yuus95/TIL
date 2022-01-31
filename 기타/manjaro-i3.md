## manjaro i3 설치 패키치


```
## sound 설치
yay -S pulseaudio-alsa
sudo pacman -Sy pa-applet
sudo pacman -Sy pavucontrol
sudo pacman -Sy pavucontrol

## 모니터연결하기
sudo mhwd -a pci nofree 0300 //nvidea driver설치
// reboot 
mhwd -li //확인하기
xrand // 연결된 모니터 확인하기
xrandr --output DP-1-1 --right-of eDP-1 --auto // edp-1 옆에 dp-1-1을 연결한다는 뜻


## 도커설치하기
yay -S docker //도커설치하기

##도커 접근권한풀기 // 처음설치할 때 접근이 없어서 docker ps 접근제한
sudo usermod -aG docker $USER
 newgrp docke

## git 설치 
yay -S git 

```