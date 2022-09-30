---
layout: post
title:  Lokal Apt Deposu ve Deb Paketi Oluşturma
---


## Gereklilikler

###  - Apache Web Sunucusu kurulumu
###  - Golang kurulumu
###  - Lokal-Repo ve Client olarak kullanacağımız iki Ubuntu sürümü.

```
  sudo apt-get install apache2
  systemctl status apache2.service
```


![GitHub Logo](/img/apache.png)

```
sudo apt-get install golang
```

![GitHub Logo](/img/go.png)

Şimdi editörümüzü açıp basit bir go programı oluşturalım ve çalıştıralım.

```
sudo nano hello.go
```

![GitHub Logo](/img/hello-world.png)

```
go run hello.go
```

![GitHub Logo](/img/run.png)

```
go build hello.go
```

![GitHub Logo](/img/hello-world.png)

```
./hello
```

![GitHub Logo](/img/hello1.png)

`ls` dediğimizde binary dosyamızın oluştuğunu görüyoruz.

![GitHub Logo](/img/ls.png)

 ```
 mkdir -p helloworld/DEBIAN`
 mkdir -p helloworld/etc/systemd/system
 sudo nano helloworld/DEBIAN/control
```

Paket version vs. bilgilerimizin tutulacağı control dosyasını oluşturalım.

![GitHub Logo](/img/control.png)

Binary dosyamızı gerekli dizine kopyaladık ve sistem servisi haline getirdik.

```
 cp hello helloworld/usr/bin/helloworld
 cp helloword.service helloworld/etc/systemd/system/helloworld.service

```

 Şimdi uygulamamızı deb paketi haline getirelim.

```
  dpkg-deb --build helloworld
  dpkg-scanpackages .
  dpkg-scanpackages . | gzip -c9  > Packages.gz
```
![GitHub Logo](/img/debpaketi.png)

![GitHub Logo](/img/hazır.png)

Paketimiz .deb hale geldi.Şimdi yapacağımız işlemleri `sudo su` ile root olarak yapalım.

 ```
    cp Packages.gz /var/www/html/debian/
    cp helloworld.deb /var/www/html/debian/
 ```
Böylelikle paketlerimizi web sunucumuzda sunmuş olduk.

## Deb paketine apt depomuzdan ulaşma

İstemcimizin source.list'ine depomuzu ekleyelim.

 ```
 sudo nano /etc/apt/sources.list
 deb [trusted=yes] http://localhost/debian ./
 ```

![GitHub Logo](/img/sourcelist.png)

 ```
 sudo apt update
 sudo apt install helloworld
 helloworld

 ```

![GitHub Logo](/img/bitti.png)

 Son olarak uygulamımızda değişiklik yaptğımızda tekrardan build edip, control dosyasında versiyonunu değiştirip deb paketini güncelleyip tekrar repomuza attığımızda, client makinemizde;

```
sudo apt update
sudo apt upgrade

```

dediğimizde uygulamımızın yeni versiyonu olduğunu görüyoruz. Uygulamamızı repomuzdan güncelleyebiliriz.

![GitHub Logo](/img/enson.png)



