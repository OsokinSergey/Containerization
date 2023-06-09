# **Контейнеризация (семинары)**
## Урок 1. Механизмы пространства имен
**Задание:** необходимо реализовать изоляцию одного и того же приложения (командного интерпретатора bash) в различных пространствах имен.

**Решение** 
#### **1. Изоляция посредством chroot.**

 Для этой реализации необходимо создать каталог, который будет становится корневым. Затем создать в нем каталог `bin` и скопировать туда `bash`. Для работы также необходимы связанные библиотеки, которые необходимо скопировать в каталоги `lib` и `lib64`. Перечень необходимых библиотек узнаем посредством утилиты `ldd`.

```
mkdir GB
mkdir GB/lib GB/lib64 GB/bin
cp /bin/bash GB/bin
ldd /bin/bash
cp /lib/x86_64-linux-gnu/libc.so.6 GB/lib
cp /lib/x86_64-linux-gnu/libtinfo.so.6 GB/lib
cp /lib64/ld-linux-x86-64.so.2 GB/lib64
chroot GB
```
Результат на рисунке.
![](images/pic1.png)

При попытке выполнения утилиты ls получаем ошибку. Для ее устранения выполним аналогичные действия, но уже для утилиты ls. Для удобства будет выолнять их в новом окне терминала.

```
cp /bin/ls GB/bin
ldd /bin/ls
cp /lib/x86_64-linux-gnu/libpcre2-8.so.0 GB/lib
cp /lib/x86_64-linux-gnu/libselinux.so.1 GB/lib
```
![](images/pic2.png)
![](images/pic3.png)



#### **2. Сетевая изоляция.**

```
ip netns list
ip netns add netisol
ip netns list
ip netns exec netisol bash
ip a
```
![](images/pic4.png)
![](images/pic5.png)

#### **3. Настраиваемая изоляция.**
![](images/pic6.png)
