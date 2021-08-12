# 一个简单的rom解包-打包

## 需要的依赖
只在ubuntu20.04用过，需要依赖python3，ubuntu20.04上有自带Python 3.8.10版本，可以通过下面命令查看
```
python3 --version
```

## 拉取项目
先拉取文件到本地
```sh
git clone https://github.com/a6680/rom.git
```
进入目录
```sh
cd rom
```
## 开始解包
将 “system.transfer.list” 和 “system.new.dat” 文件放入 “rom” 目录
将system.new.dat.br转为system.new.dat，并删除源文件，如果不想删除源文件可以把 “j” 去掉
```sh
~/rom/bin/brotli -dj system.new.dat.br
```
system.new.dat转为system.img
```sh
python3 ~/rom/bin/sdat2img.py system.transfer.list system.new.dat
```
在/mnt/内创建system，用来挂在system.img
```
sudo mkdir /mnt/system
```
挂载system.img
```sh
sudo mount system.img /mnt/system
```
到这里算是挂载好了，可以修改system.img内的文件了。

## 打包文件
取消system.img挂载
```sh
sudo umount /mnt/system
```
将system.img转为system.new.dat
```sh
python3 ~/rom/bin/rimg2sdat.py system.img
```
将system.new.dat转为system.new.dat.br，中间的0是压缩级别，可选(0-9)
```sh
~/rom/bin/brotli -0 system.new.dat
```

