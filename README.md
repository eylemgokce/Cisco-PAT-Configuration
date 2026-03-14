Cisco Packet Tracer - PAT (NAT Overload) Yapılandırması

Bu proje, Cisco Packet Tracer üzerinde PAT (Port Address Translation) kullanarak özel bir yerel ağın (LAN) tek bir genel IP adresi üzerinden dış ağlara (Internet/ISP) nasıl çıkarılacağını adım adım göstermektedir.
🛠 Topoloji Bilgileri

    İç Ağ (LAN): 192.168.1.0 /24

    Dış Bağlantı (WAN): 10.0.0.0 /24 (R1 ve ISP arası)

    Sunucu Ağı: 192.31.7.0 /24

    Kullanılan Cihazlar: 2x 1941 Router, 1x 2960 Switch, 2x PC, 1x Server.

🚀 Uygulama Adımları
1. Arayüzlerin NAT Rollerinin Belirlenmesi

Router R1 üzerinde iç ve dış bacaklar tanımlanmıştır:

R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip nat inside
R1(config)# interface serial 0/1/0
R1(config-if)# ip nat outside

2. Erişim Listesi (ACL) Oluşturma

Sadece yerel ağımızdaki cihazların internete çıkabilmesi için standart bir ACL tanımlanmıştır:

R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255

3. PAT (Overload) Yapılandırması

ACL ile belirlenen trafiğin, dış arayüz IP'sini (10.0.0.1) port bazlı paylaşması sağlanmıştır:

R1(config)# ip nat inside source list 1 interface serial 0/1/0 overload

4. Yönlendirme (Routing)

Paketlerin ISP tarafına ulaşabilmesi için varsayılan rota (default route) eklenmiştir:
R1(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.2

🔍 Doğrulama ve Test

Yapılandırmayı test etmek için PC'lerden dış ağdaki sunucuya (192.31.7.100) ping atılmış ve NAT tablosu kontrol edilmiştir.

NAT Çeviri Tablosu Sorgulama:

R1# show ip nat translations

Öğrenilenler

    Özel IP adreslerinin (Private IP) neden internete doğrudan çıkamadığı.

    overload komutunun portlar aracılığıyla nasıl IP tasarrufu sağladığı.
