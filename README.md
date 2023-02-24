# Swap Dosyaları ve Swap Ekleme

## Swap Alanını Açacağınız Diskin Özellikleri

Yüksek okuma ve yazma hızına sahip NVME M.2 SSD'ler en iyi performansı
verecektir. Sata SSD'ler ikinci seçenek olarak kullanılabilir. Genelde hızları
NVME M.2 SSD'ler kadar yüksek değildir. HDD'ler üçüncül tercihiniz olabilir.
Tek şansınız bu ise yine de eklemekte yarar olabilir. Taşınabilir cihazlar
bağlantı kopma ihtimaline karşı en son tercihiniz olmalıdır.

## Ram ve Swap Performans Karşılaştırması

Kendi elimdeki Samsung 980 NVME M.2 SSD ile Ripjaws DDR4 3200 Mhz RAM
karşılaştırıldığında RAM 33.8 GB/s okuma hızına sahipken SSD 3.5 GB/s hıza
sahip. Basit bir oranla 9.65 gibi bir hız değişimi var diyebiliriz. En iyi
seçenek olan NVME M.2 SSD'ler bile yaklaşık 10 kat yavaş kalıyor. Dolayısıyla
Swap, RAM'ın yerini tutamasa da ona destek olma konusunda kullanışlıdır.
Özellikle yüksek okuma yazma hızına ihtiyaç duymayacağınız alanlarda RAM
olarak kullanabilirsiniz.

## Swap ve RAM kontrolü

```sh
free --mega -t
```

## Swap Alanına Karar Verin

Aşağıdaki gibi, kullanıma göre karar verebilirsiniz.

* 16 GB RAM + 24 GB Swap, Video düzenleme
* 16 GB RAM + 8 GB Swap, Oyun
* 8 GB RAM + 4 GB Swap, Normal kullanım
* ...

## Swap Çeşitleri

Swap, disk bölümü olarak da ayarlanabilir sistemdeki bir dosya
olarak da. Sistemi kurarken Swap partition olarak ekleme yapmak
mantıklıdır. Ancak sistemi kurduktan sonra partition table ile
oynamak istemezseniz dosya olarak da ekleyebilirsiniz. Hem üzerinde
oynama yapmak da daha kolay olmaktadır.

## Swap Dosyası Boyutunu Hesaplama

Blok boyutu 1024 olan 1024 blok bize bit GB alan oluşturur. Bu
nedenle aşağıdaki formülle gerekli alanı hesaplayabilirsiniz.

```
1024*1024*[GB cinsinden istediğiniz alan]
```

Benim durumumda bu alan 25165824 olacaktır.

## Dosyayı Oluşturma

Aşağıdaki komut `24 GB` boyutunda, (girdi dosyası `/dev/zero` olduğu için) 0
bitleri ile dolu `/swapfile` dosyasını oluşturur.

```sh
dd if=/dev/zero of=/swapfile bs=1024 count=25165824
```

### Dosya İzinlerini Ayarlama

Dosyanın kullanılabilmesi için yalnızca root kullanıcısına okuma ve yazma
izinlerini (4+2) vermeliyiz.

```sh
chmod 600 /swapfile
```

### Dosyayı Swap Haline getirme

Sanırım her şey açık.

```sh
mkswap /swapfile
```

### Swap Dosyasını Tek Seferlik Aktif Etme

Tek seferlik bunu yapıyoruz çünkü otomatik olarak aktif etme konfigürasyonunu
yaptığımızda yeniden başlatmadan swap aktif olmayacak.

```sh
swapon /swapfile
```

### Swap Dosyasını Fstab Dosyasına Kaydetme

Dosya sistemi tablosuna (fstab) bu dosyayı eklemeliyiz ki bilgisayarı
açtığınızda otomatik olarak swap aktif edilsin.

```sh
echo "/swapfile     swap     swap    defaults    0 0" >> /etc/fstab
```

Bu komut yerine bir metin düzenleme aracı ile de aşağıdaki metni `/etc/fstab` dosyasına yapıştırabilirsiniz.

```
/swapfile     swap     swap    defaults    0 0
```

## Tüm Swap Dosyalarını Aktif/Deaktif Etme

Aşağıdaki komut ile tüm swap dosyalarını deaktif edebilirsiniz.

```sh
swapoff -a

```

Aşağıdaki ile ise tümünü aktif edebilirsiniz.


```sh
swapon -a

```
