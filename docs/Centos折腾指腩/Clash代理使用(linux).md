##1、在自己工作账户目录下
```
cd && mkdir clash
wget https://github.com/Dreamacro/clash/releases/download/v1.8.0/clash-linux-amd64-v1.8.0.gz
gzip -d clash-linux-amd64-v1.8.0.gz
chmod +x clash-linux-amd64-v1.8.0
mv clash-linux-amd64-v1.8.0 clash
```
一般linux下载clash-linux-amd64.tar.gz 就可以
clash的github地址：https://github.com/Dreamacro/clash/releases

##2、下载配置yaml文件
```
cd clash
wget -O config.yaml https://****你的yaml配置文件地址 
```

##3、启动执行
nohup ./clash -d . >> clash.out 2>&1 &

##4、页面配置
http://clash.razord.top/
![image.png](https://upload-images.jianshu.io/upload_images/15108341-df72964466ac303d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
左下角会出现你的clash版本号

##5、导入配置
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890

7890是我的代理的端口，9090是config.yaml里面，restful api指定的端口
记得去云服务器把端口开一下 不然访问不了
