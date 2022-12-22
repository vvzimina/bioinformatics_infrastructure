# bioinformatics_infrastructure
Repo for bioinformatics infrastructure homework

## Theory [2]

* [0.4] What are [computer ports](https://www.cloudflare.com/learning/network-layer/what-is-a-computer-port/) on a high level? How many ports are there on a typical computer?
Порт - это конкретный адрес приема/отправки информации в сеть в пределах одного устройства. Если наш компьютер - это многоквартирный дом (его адрес = IP адрес), то порт означает номер конкретной двери. Порты могут отличаться разными сетевыми протоколами.
The numbers go from 0 to 65535, which is a 16-bit number => 65536 total ports

* [0.4] What is the difference between http, https, ssh, and other protocols? In what sense are they similar? Name default ports for several data transfer protocols.
http - протокол для передачи гипертекста, https - он же, но более безопасный за счет использования шифрованного канала. SSH еще более безопасный протокол, который шифрует всю информацию (сами данные могут быть любыми), используется для передачи паролей и конфиденциальной информации, удаленным управлением других устройств. Разница между протоколами может быть в том, для каких типов данных они предназначены, насколько гарантируют доставку данных, как и на каком этапе защищают данные или канал передачи.

http - 80
https - 443
ssh - 22
smtp - 25
dns - 53

* [0.4] Explain briefly: (1) what is IP, (2) what IPs are called 'white'/public, (3) and what happens when you enter 'google.com' into the web browser. 
(1) IP is an address (identifier) that allows information to be sent between devices on a network: they contain location information and make devices accessible for communication. **СКОПИРОВАНО с https://www.kaspersky.com/resource-center/definitions/what-is-an-ip-address
(2) Так как общее количество IP адресов ограничено (4,294,967,296 for IPv4) и меньше, чем устройств, то по умолчанию устройство не имеет белого/публичного/фиксированного IP адреса. Но если кто-то хочет получить "прописку" в сети, то он должен оформить себе белый=официальный IP, который будет занесен в реестр.
(3) 'google.com' is the domain name of the site. It is the memorable address and points to a specific server’s IP address. **СКОПИРОВАНО из https://aws.amazon.com/ru/blogs/mobile/what-happens-when-you-type-a-url-into-your-browser/
-> Browser looks up IP address for the domain (the browser needs to figure out which server on the Internet to connect to. To do that, it needs to look up the IP address of the server hosting the website using the domain you typed in)
-> Browser initiates TCP connection with the server and data moving between you and specified server

* [0.4] What is Nginx? How does it work on the high level? List several alternative web servers.
это программа для развертывания собственного веб-сервера. Может использоваться как независимый полноценный сервер, так и прокси с переадресацией веб-запросов на другие сервера.
Этот сервис использует открытый исходный код и доступен любому для редактирования и подстраивания под свои нужды.
Когда пользователь совершает действие на странице, информация передается на сервер. Он находит файлы, передает сведения. Запросы от одного пользователя разбиваются на маленькие структуры — сетевые соединения. Обработка происходит быстрее: за однотипные действия отвечает один процесс. После обработки соединения собираются в одном виртуальном контейнере, чтобы преобразоваться в единый первоначальный запрос, а затем отправляются пользователю.
Apache
Tomcat
LiteSpeed

* [0.4] What is SSH, and for what is it typically used? Explain two ways to authenticate in an SSH server in detail.
SSH or Secure Shell is a network communication protocol that enables two computers to communicate (c.f http or hypertext transfer protocol, which is the protocol used to transfer hypertext such as web pages) and share data. An inherent feature of ssh is that the communication between the two computers is encrypted meaning that it is suitable for use on insecure networks.

SSH is often used to "login" and perform operations on remote computers but it may also be used for transferring data.
**СКОПИРОВАНО из https://www.ucl.ac.uk/isd/what-ssh-and-how-do-i-use-it

