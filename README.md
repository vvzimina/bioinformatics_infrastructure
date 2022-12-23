# bioinformatics_infrastructure
Repo for bioinformatics infrastructure homework

## Theory [2]

* [0.4] What are [computer ports](https://www.cloudflare.com/learning/network-layer/what-is-a-computer-port/) on a high level? How many ports are there on a typical computer?

Порт - это конкретный адрес приема/отправки информации в сеть в пределах одного устройства. Если наш компьютер - это многоквартирный дом (его адрес = IP адрес), то порт означает номер конкретной двери. Порты могут отличаться разными сетевыми протоколами.
Номера портов бывают от 0 до 65535, так как записываются 16-битным числом => 65536 портов всего.

* [0.4] What is the difference between http, https, ssh, and other protocols? In what sense are they similar? Name default ports for several data transfer protocols.

http - протокол для передачи гипертекста, https - он же, но более безопасный за счет использования шифрованного канала. SSH еще более безопасный протокол, который шифрует всю информацию (сами данные могут быть любыми), используется для передачи паролей и конфиденциальной информации, удаленным управлением других устройств. Разница между протоколами может быть в том, для каких типов данных они предназначены, насколько гарантируют доставку данных, как и на каком этапе защищают данные или канал передачи.

http - 80; 
https - 443; 
ssh - 22; 
smtp - 25; 
dns - 53; 

* [0.4] Explain briefly: (1) what is IP, (2) what IPs are called 'white'/public, (3) and what happens when you enter 'google.com' into the web browser. 

(1) IP - это адрес (идентификатор) который  позволяет обмениваться информацией между устройствами в сети. Такой адрес содержит в себе информацию о местонахождении и устроуйства и позволяет найти его в сети.

(2) Так как общее количество IP адресов ограничено (4,294,967,296 для IPv4) и меньше, чем устройств, то по умолчанию устройство не имеет белого/публичного/фиксированного IP адреса. Но если кто-то хочет получить "прописку" в сети, то он должен оформить себе белый=официальный IP, который будет занесен в реестр.

(3) 'google.com' это доменное имя сайта. Оно "красивое/человекочитаемое" имя для конкретного IP адреса сервера или прочего устройства.

-> После нажатия 'enter' браузер ищет IP адрес домена, чтобы позднее организовать коннект до сервера. Для этого бразуер смотрит справочник доменов-адресов у себя в памяти или, если не находит, запрашивает на DNS сервере.

-> Браузер инициирует TCP протокол соединения (так как https редполагает именно этот протокол) с сервером и они начинают обмениваться данными.

* [0.4] What is Nginx? How does it work on the high level? List several alternative web servers.

Это программа для развертывания собственного веб-сервера. Может использоваться как независимый полноценный сервер, так и прокси (доверенный адрес) с переадресацией веб-запросов на другие сервера. Этот сервис использует открытый исходный код и доступен любому для редактирования и подстраивания под свои нужды.

Когда пользователь совершает действие на странице, информация передается на сервер. Он находит файлы, передает сведения. Запросы от одного пользователя разбиваются на маленькие структуры — сетевые соединения. Обработка происходит быстрее: за однотипные действия отвечает один процесс. После обработки соединения собираются в одном виртуальном контейнере, чтобы преобразоваться в единый первоначальный запрос, а затем отправляются пользователю.

Apache, Tomcat, LiteSpeed

* [0.4] What is SSH, and for what is it typically used? Explain two ways to authenticate in an SSH server in detail.

SSH или Secure Shell это сетевой протокол, который позволяет двум компьютерам взаимодействовать и обменивать данными. В отличии от прочих протоколов SSH концентрируется на безопасности и позволяет настраивать защищенное соединение, где канал и данные шифруются.

SSH часто используется для удаленного управления компьютерами, но может использоваться и для передачи обычных данных.

В основном для авторизации на SSH сервере используют пароль или ключ.

Для подключения через пароль нам необходимо знать IP адрес сервера, логин, пароль.
1. Входим в терминал.
2. Вводим ssh root@xxx.xxx.xxx.xx -p yyy. root - наш логин. После @ идет IP-адрес сервера. Затем с помощью -p указывает порт. Но соединение может работать и без указания конкретного порта, если мы пользуемся стандартным (22).
3. У нас запрашивает пароль, мы вводим. YES при первом входе на устройство.
4. Готово.

Более безопасным считается способ авторизации через ключ, так как пароль можно подобрать. Для использования этого метода генерится два SSH ключа: публичный и приватный. Первый отдается серверу, второй хранится на устройстве. При попытке подключения ключи взаимодействуют друг с другом и открывают доступ.
1. Используем терминал.
2. Вводим ssh-keygen для генерации ключей.
3. Теперь копируем публичную часть ключа на сервер, для этого используем команду ssh-copy-id root@xxx.xxx.xxx.xx (root - наш логин, после @ идет IP адрес сервера).
4. Теперь при входе через ssh root@xxx.xxx.xxx.xx мы сразу попадем на сервер и больше ничего вводить не придется.

## Problem [6.5]
**Remote Server**:
* [2] Create a new virtual machine in the Yandex/Mail/etc cloud (order at least 10GB of free disk space). Generate SSH key pair and use it to connect to your server.

1. Сначала создаем SSH ключи. **ssh-keygen -b 4096 -t rsa**.
2. Сохраняем в стандартную папку. Кодовую фразу (не) задаем.
3. Получаем публичный ключ. **cat .ssh/id_rsa.pub**.
4. Создаем аккаунт на ЯОблаке. Создаем платежный аккаунт с пробным периодом.
5. Создаем виртуальную машину. Ubuntu 22.04, hdd 15gb, 2 cpu intel ice lake, 2gb ram, публ. ip автоматически
6. Создаем сервисный аккаунт, сообщаем наш ssh публичный ключ из п.3. Задаем логин.
7. Дожидаемся статуса running для ВМ.

![image](https://user-images.githubusercontent.com/121238982/209333500-6736426b-dc9c-48eb-b08d-38d3385579c0.png)

8. В терминале вводим ssh vika@158.160.22.16 и заходим на ВМ.

![image](https://user-images.githubusercontent.com/121238982/209333743-a67324fd-7fe4-4fbd-ae9f-8d5d7c979e1e.png)

* [1] Download the latest human genome assembly (GRCh38) from the Ensemble FTP server ([fasta](https://ftp.ensembl.org/pub/release-108/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz), [GFF3](https://ftp.ensembl.org/pub/release-108/gff3/homo_sapiens/Homo_sapiens.GRCh38.108.gff3.gz)). Index the fasta using samtools (`samtools faidx`) and GFF3 using tabix. 
* [1] Select and download BED files for three ChIP-seq and one ATAC-seq experiment from the ENCODE (use one tissue/cell line). Sort, bgzip, and index them using tabix.

**JBrowse 2**
* [1] Download and install [JBrowse 2](https://jbrowse.org/jb2/). Create a new jbrowse [repository](https://jbrowse.org/jb2/docs/cli/#jbrowse-create-localpath) in `/mnt/JBrowse/` (or some other folder).
* [0.25] Install nginx and amend its config(/etc/nginx/nginx.conf) to contain the following section:
```conf
http {
  # Don't touch other options!
  # ........
  # ........

  # Comment this line(!):
  # include /etc/nginx/sites-enabled/*;

  # Add this:
  server {
    listen 80 default_server;
    index index.html;
    server_name _;

    location /jbrowse/ {
      alias /mnt/JBrowse/;	
    }
  }
}
```

* [0.25] Restart the nginx (reload its config) and make sure that you can access the browser using a link like this: `http://64.129.58.13/jbrowse/`. Here `64.129.58.13` is your public IP address.
* [1] Add your files (BED & FASTA & GFF3) to the genome browser and verify that everything works as intended. Don't forget to [index](https://jbrowse.org/jb2/docs/cli/#jbrowse-text-index) the genome annotation, so you could later search by gene names. Provide a [persistent link](https://jbrowse.org/jb2/docs/user_guides/basic_usage/#sharing-sessions) to a JBrowse 2 session with all your BED files and the genome annotation in the report (like [this](https://jbrowse.org/code/jb2/v2.3.1/?session=share-HShsEcnq3i&password=nYzTU)). *I must be able to access it without problems later.*


**Common mistakes**:
* Using `/home/username` folder for JBrowse. Don't do this - you will have permission issues (403 forbidden) because by default home is only available to your user, not to the nginx user(group).
* No trailing `/` in the config (`/jbrowse/`, `/mnt/JBrowse/`), or in the URL (`http://64.129.58.13/jbrowse/`).
* If you have added tracks but they are not showing up in JBrowse - try reloading the page or use a private/incognito window.
