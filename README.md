# История и достоинство
> Internet Protocol Security (IPsec) — это набор протоколов для обеспечения защиты данных, передаваемых по IP-сети. В отличие от SSL, который работает на прикладном уровне, IPsec работает на сетевом уровне и может использоваться нативно со многими операционными системами, что позволяет использовать его без сторонних приложений.

IPsec шифрует весь IP-пакет, используя:

Authentication Header (AH), который ставит цифровую подпись на каждом пакете;
Encapsulating Security Protocol (ESP), который обеспечивает конфиденциальность, целостность и аутентификацию пакета при передаче.

Обсуждение IPsec было бы неполным без упоминания утечки презентации Агентства Национальной Безопасности США, в которой обсуждаются протоколы IPsec (L2TP и IKE). Трудно прийти к однозначным выводам на основании расплывчатых ссылок в этой презентации, но если модель угроз для вашей системы включает целевое наблюдение со стороны любопытных зарубежных коллег, это повод рассмотреть другие варианты. И все же протоколы IPsec еще считаются безопасными, если они реализованы должным образом.

Layer 2 Tunneling Protocol (L2TP) был впервые предложен в 1999 году в качестве обновления протоколов L2F (Cisco) и PPTP (Microsoft). Поскольку L2TP сам по себе не обеспечивает шифрование или аутентификацию, часто с ним используется IPsec. L2TP в паре с IPsec поддерживается многими операционными системами, стандартизирован в RFC 3193.


L2TP/IPsec считается безопасным и не имеет серьезных выявленных проблем (гораздо безопаснее, чем PPTP). L2TP/IPsec может использовать шифрование 3DES или AES, хотя, учитывая, что 3DES в настоящее время считается слабым шифром, он используется редко.


У протокола L2TP иногда возникают проблемы из-за использования по умолчанию UDP-порта 500, который, как известно, блокируется некоторыми брандмауэрами.


Протокол L2TP/IPsec позволяет обеспечить высокую безопасность передаваемых данных, прост в настройке и поддерживается всеми современными операционными системами. Однако L2TP/IPsec инкапсулирует передаваемые данные дважды, что делает его менее эффективным и более медленным, чем другие VPN-протоколы.
