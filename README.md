# История и достоинствa

## История и достоинства IPSec
Еще в 1992 году IETF (internet Engineering Task Force) начал разрабатывать протокол IPSec, а точнее целове семейство протоколов. Поскольку в то время очень оживленно продвигали протокол Ipv6, идея внедрить в него возможность криптографической защиты данных на сетевом уровне казалась довольно удачной. Таким образом в 1995 году появились два протокола Authentification Header (AH) и Encapsulating Security Payload (ESP), которые преполагали использовать в связке. AH обеспечивал аутентификацию и целостность IP пакета, ESP обеспечивал его конфиденциальность. В 1998 году эти протоколы обновили и возоможности аутентификации и целостности были внедрены в ESP, таким образом в большинстве случаев отпала необходимость использования связки ESP + AH.
Позже в ESP добавили инкапсуляцию UDP чтобы данный протокол был совместим с трансляторами сетевых адресов (NAT), что через некоторое время привело к исчезновению AH за ненадобностью. Последняя ревизия AH и ESP была сделана в 2005 году и с того момента протоколы остаются неизменными, а их основа сохраняется с 1998 года. 

> Internet Protocol Security (IPsec) — это набор протоколов для обеспечения защиты данных, передаваемых по IP-сети. В отличие от SSL, который работает на прикладном уровне, IPsec работает на сетевом уровне и его можно использовать нативно со большинством операционных системам, что позволяет использовать его без сторонних приложений.

IPsec шифрует весь IP-пакет, используя:

1. Authentication Header (AH), который ставит цифровую подпись на каждом пакете;
2. Encapsulating Security Protocol (ESP), который обеспечивает конфиденциальность, целостность и аутентификацию пакета при передаче.

## История и достоинства L2TP
Layer 2 Tunneling Protocol (L2TP) был впервые предложен в 1999 году в качестве обновления протоколов L2F (Cisco) и PPTP (Microsoft). Поскольку L2TP сам по себе не обеспечивает шифрование или аутентификацию, часто с ним используется IPsec. L2TP в паре с IPsec поддерживается многими операционными системами, стандартизирован в RFC 3193.

L2TP/IPsec считается безопасным и не имеет серьезных выявленных проблем (гораздо безопаснее, чем PPTP). L2TP/IPsec может использовать шифрование 3DES или AES, хотя, учитывая, что 3DES в настоящее время считается слабым шифром, он используется редко.

У протокола L2TP иногда возникают проблемы из-за использования по умолчанию UDP-порта 500, который, как известно, блокируется некоторыми брандмауэрами.

Протокол L2TP/IPsec позволяет обеспечить высокую безопасность передаваемых данных, прост в настройке и поддерживается всеми современными операционными системами. Однако L2TP/IPsec инкапсулирует передаваемые данные дважды, что делает его менее эффективным и более медленным, чем другие VPN-протоколы.

# Описание и структура L2TP пакета
L2TP использует протокол UDP как транспорт, а так же использует для пересылки данных такой же формат сообщений, как и для управления туннелем. В реализации Microsoft L2TP использует как контрольные сообщения UDP пакеты, которые содержат зашифрованные пакеты PPP.

Чтобы обеспечить безопасность L2TP-пакетов чаще всего используется протокол IPSec, который обеспечивает конфиденциальность, целостность и аутентификацию. Связка этих протокол известна как L2TP/IPSec.

Конечных узлов L2TP туннеля два. Один из них LAC (L2TP Access Controller) является инициатором туннеля. Другой LNS (L2TP Network Server) - это сервер, который ожидает появление новых туннелей. Когда туннель установлен, трафик между узлами направлен в обе стороны. После установления туннеля, внутри него запускаются протоколы более высоких уровней. С этой целью, L2TP сессия устанавливается внутри туннеля для каждого протокола более высокого уровня. (например, для PPP). И LAC, и LNS могут инициировать сессии. Трафик для каждой из сессий изолируется с помощью L2TP, таким образом появляется возможность настроить несколько виртуальных сетей в одном туннеле.

У энкапсуляции L2TP/IPSec 3 этапа:
* Инкапсуляция L2TP. Кадр PPP (IP датаграмма или IPX-датаграмма) заключается в оболочку с заголовком L2TP и UDP.
* Икапсуляция IpSec. После этого полученное L2TP сообщение заключается в оболочку с заголовком и трейлером IPSec ESP (Encapsulating Security Payload), который обеспечивает целостность и подлинность сообщения и ip заголовок. В заголовке IP-адреса источника и приемника соответствуют VPN-клиенту и VPN-серверу.
* Вторая инкапсуляция L2TP. После этих этапов L2TP выполняет вторую инкапсуляцию PPP чтобы подготовить данные к передаче.

L2TP пакет состоит из :
* Флагов, указывающих один из двух типов пакета (управляющий или данные) и наличие полей с длиной, номером последовательности и сдвигом - **(Flags and version)**
* Полной длины сообщения в байтах, есть только когда установлен флаг длины. - **(Length)**
* Идентификатора управляющего соединения - **(Tunnel ID)**
* Идентификатора сессии внутри туннеля - **(Session ID)**
* Последовательного номера для данных или управляющего сообщения, начиная с 0 и увеличиваясь на единицу (по модулю 216) для каждого отправленного сообщения. Существует, когда установлен соответствующий фла - **(Ns)**
* Последовательного номера сообщения, ожидаемого для принятия. Nr устанавливается как Ns последнего полученного сообщения плюс один (по модулю 216). В сообщениях с данными, Nr зарезервирован, и, если существует, ОБЯЗАН быть проигнорирован - **(Nr)**
* Размера смещения, который определяет, где находятся данные после L2TP заголовка. Если это поле существует, L2TP заголовок заканчивается после последнего байта offset padding. Существует, когда установлен соответствующий флаг - **(Offset Size)**
* Переменной длины, указывается offset size. Содержимое поля неопределенно - **(Offset Pad)** 
* Самих передаваемых данных. Переменной длины (Максимальный размер = Максимальному размеру UDP пакета — размер заголовка L2TP) - **(Payload data)**

