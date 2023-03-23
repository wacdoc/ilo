# Mangbangon ti bukodmo nga SMTP mail sending server

## preambulo

Ti SMTP ket direkta a makagatang kadagiti serbisio manipud kadagiti aglaklako ti ulep, a kas ti:

* [Amazon SES nga SMTP](https://docs.aws.amazon.com/ses/latest/dg/send-email-smtp.html)
* [Ali ulep nga email push](https://www.alibabacloud.com/help/directmail/latest/three-mail-sending-methods)

Mabalinmo pay ti mangbangon ti bukodmo a mail server - awan limitasionna a panagipatulod, nababa ti pakabuklan a gastos.

Iti baba, ipakitami ti addang nga addang no kasano ti mangbangon iti bukodmi a mail server.

## Panagpili ti serbidor

Ti bukod a naisangayan nga SMTP a serbidor ket agkasapulan ti publiko nga IP nga addaan kadagiti puerto 25, 456, ken 587 a silulukat.

Dagiti kadawyan a maus-usar a publiko nga ulep ket nanglapped kadagitoy a puerto babaen ti default, ken mabalin a mabalin a luktan dagitoy babaen ti panangipaulog ti bilin ti trabaho, ngem daytoy ket makariribuk unay kalpasan amin.

Irekomendak ti gumatang manipud iti host nga addaan kadagitoy a puerto a silulukat ken mangsuporta iti panangisaad kadagiti baliktad a nagan ti domain.

Ditoy, irekomendak [ti Contabo](https://contabo.com) .

Ti Contabo ket maysa a hosting provider a nakabase idiay Munich, Alemania, a naipasdek idi 2003 nga addaan iti nasalisal unay a presio.

No piliem ti Euro kas kuarta ti panaggatang, nalaklaka ti presio (ti server nga addaan iti 8GB a memoria ken 4 a CPU ket aggatad iti agarup 529 yuan iti kada tawen, ken libre ti umuna a bayad ti pannakaipasdek iti maysa a tawen).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/UoAQkwY.webp)

No mangikabil ti order, remark `prefer AMD` , ken ti server nga addaan iti AMD CPU ket addaanto iti nasaysayaat a panagaramid.

Iti sumaganad, alaek ti VPS ti Contabo kas pagarigan tapno maipakita no kasano ti mangbangon iti bukodmo a mail server.

## Konfigurasion ti sistema ti Ubuntu

Ti sistema ti panagpataray ditoy ket ti Ubuntu 22.04

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/smpIu1F.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/m7Mwjwr.webp)

No ti server iti ssh ket mangiparang `Welcome to TinyCore 13!` (kas naipakita iti ladawan iti baba), kayatna a sawen a saan pay a naikabil ti sistema. Pangngaasiyo ta i-disconnect ti ssh ken urayenyo ti sumagmamano a minuto tapno makalog-in manen.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/-qKACz9.webp)

No agparang `Welcome to Ubuntu 22.04.1 LTS` , kompleto ti panangrugi, ken mabalinmo nga ituloy dagiti sumaganad nga addang.

### [Opsional] Irugi ti aglawlaw ti panagdur-as

Opsional daytoy nga addang.

Para iti kombeniente, inkabilko ti panagipasdek ken panagisaad ti sistema ti software ti ubuntu iti [github.com/wactax/ops.os/tree/main/ubuntu](https://github.com/wactax/ops.os/tree/main/ubuntu) .

Ipataray ti sumaganad a bilin tapno mai-install babaen ti maysa a panagpidut.

```
bash <(curl -s https://raw.githubusercontent.com/wactax/ops.os/main/ubuntu/boot.sh)
```

Dagiti agar-aramat nga Insik, pangngaasiyo ta usarenyo ketdi ti sumaganad a bilin, ket automatiko a maikeddeng ti pagsasao, sona ti oras, kdpy.

```
CN=1 bash <(curl -s https://ghproxy.com/https://raw.githubusercontent.com/wactax/ops.os/main/ubuntu/boot.sh)
```

### Ti Contabo ket mangpabalin ti IPV6

Pagbalinen ti IPV6 tapno ti SMTP ket makaipatulod pay kadagiti email nga addaan kadagiti pagtaengan ti IPV6.

i-edit ti `/etc/sysctl.conf`

Baliwan wenno inayon dagiti sumaganad a linia

```
net.ipv6.conf.all.disable_ipv6 = 0
net.ipv6.conf.default.disable_ipv6 = 0
net.ipv6.conf.lo.disable_ipv6 = 0
```

Suroten [ti contabo tutorial: Panagnayon ti koneksion ti IPv6 iti serbidormo](https://contabo.com/blog/adding-ipv6-connectivity-to-your-server/)

Urnosen ti `/etc/netplan/01-netcfg.yaml` , manginayon ti sumagmamano a linia a kas naipakita iti pigura iti baba (Ti Contabo VPS default configuration file ket addaanen kadagitoy a linia, basta i-uncomment dagitoy).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/5MEi41I.webp)

Kalpasanna `netplan apply` tapno agbalin nga epektibo ti nabaliwan a konfigurasion.

Kalpasan a naballigi ti panagisaad, mabalinmo nga usaren `curl 6.ipw.cn` tapno makita ti ipv6 a pagtaengan ti akinruar a networkmo.

## I-clone ti pagidulinan ti konfigurasion ops

```
git clone https://github.com/wactax/ops.soft.git
```

## Mangpataud ti libre a sertipiko ti SSL para iti nagan ti domain-mo

Ti panangipatulod ti koreo ket kasapulan ti sertipiko ti SSL para iti panagenkripsio ken panagpirma.

Usarenmi [ti acme.sh](https://github.com/acmesh-official/acme.sh) tapno mangpataud kadagiti sertipiko.

Ti acme.sh ket maysa nga open source nga automated a ramit ti panagpirma ti sertipiko, .

Iserrek ti bodega ti panagisaad ops.soft, tarayen `./ssl.sh` , ken ti maysa a folder `conf` ket maparsua iti **akinngato a direktorio** .

Sapulen ti mangipapaay ti DNS-mo manipud [iti acme.sh dnsapi](https://github.com/acmesh-official/acme.sh/wiki/dnsapi) , i-edit `conf/conf.sh` .

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/Qjq1C1i.webp)

Kalpasanna, tarayen `./ssl.sh 123.com` tapno makapataud kadagiti sertipiko `123.com` ken `*.123.com` para iti nagan ti domain-mo.

Ti umuna a panagtaray ket automatiko nga i-install [ti acme.sh](https://github.com/acmesh-official/acme.sh) ken manginayon ti naikeddeng nga aramid para iti automatiko a panagpabaro. Makitam `crontab -l` , adda ti kasta a linia a kas ti sumaganad.

```
52 0 * * * "/mnt/www/.acme.sh"/acme.sh --cron --home "/mnt/www/.acme.sh" > /dev/null
```

Ti dalan para iti napataud a sertipiko ket maysa a banag a kas ti `/mnt/www/.acme.sh/123.com_ecc。`

Ti panagpabaro ti sertipiko ket mangawag `conf/reload/123.com.sh` nga iskrip, i-edit daytoy nga iskrip, mabalinmo ti manginayon kadagiti bilin a kas ti `nginx -s reload` tapno mangpabaro ti cache ti sertipiko dagiti mainaig nga aplikasion.

## Mangbangon ti SMTP server nga addaan iti chasquid

[ti chasquid](https://github.com/albertito/chasquid) ket maysa nga open source nga SMTP server a naisurat iti pagsasao a Go.

Kas kasukat dagiti kadaanan a programa ti mail server a kas ti Postfix ken Sendmail, ti chasquid ket nasimsimple ken nalaklaka nga usaren, ken nalaklaka pay para iti sekondario a panagrang-ay.

Ti run `./chasquid/init.sh 123.com` iti maysa a panagpidut (sukatan ti 123.com iti mangipatulod a nagan ti domain-mo).

## Ikonfigura ti DKIM ti Pirma ti Email

Mausar ti DKIM a mangipatulod kadagiti pirma ti email tapno malapdan ti pannakatrato dagiti surat kas spam.

Kalpasan ti sibaballigi a panagtaray ti bilin, maibaga kenka nga ikeddeng ti rekord ti DKIM (kas naipakita iti baba).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/LJWGsmI.webp)

Inayonmo laeng ti TXT record iti DNS-mo (kas naipakita iti baba).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/0szKWqV.webp)

## Kitaen ti kasasaad ti serbisio & logs

 `systemctl status chasquid` Kitaen ti kasasaad ti serbisio.

Ti kasasaad ti normal nga operasion ket kas naipakita iti ladawan iti baba

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/CAwyY4E.webp)

 `grep chasquid /var/log/syslog` wenno `journalctl -xeu chasquid` ket mabalinna a kitaen ti log ti biddut.

## Baliwan ti konfigurasion ti nagan ti domain

Ti baliktad a nagan ti dominio ket tapno mangipalubos ti pagtaengan ti IP a marisut iti maitunos a nagan ti dominio.

Ti panangisaad iti baliktad a nagan ti domain ket mabalin a manglapped kadagiti email manipud iti pannakailasin a kas spam.

No maawat ti koreo, ti umaw-awat a serbidor ket mangaramidto ti panaganalisar ti baliktad a nagan ti dominio iti pagtaengan ti IP ti mangipatulod a serbidor tapno pasingkedan no ti mangipatulod a serbidor ket addaan iti balido a baliktad a nagan ti dominio.

No ti mangipatulod a serbidor ket awan ti baliktad a nagan ti dominio wenno no ti baliktad a nagan ti dominio ket saan a maitunos iti IP a pagtaengan ti mangipatulod a serbidor, ti umaw-awat a serbidor ket mabalin a mangbigbig ti email a kas spam wenno ilaksid daytoy.

Bisitaen [ti https://my.contabo.com/rdns](https://my.contabo.com/rdns) ken i-configure kas naipakita iti baba

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/IIPdBk_.webp)

Kalpasan ti panangisaad ti baliktad a nagan ti dominio, laglagipen nga ikonfigura ti agpasango a resolusion ti nagan ti dominio nga ipv4 ken ipv6 iti serbidor.

## Urnosen ti nagan ti host ti chasquid.conf

Baliwan `conf/chasquid/chasquid.conf` iti pateg ti baliktad a nagan ti dominio.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/6Fw4SQi.webp)

Kalpasanna, ipatarayen `systemctl restart chasquid` tapno mairugi manen ti serbisio.

## Backup ti conf iti pagidulinan ti git

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/Fier9uv.webp)

Kas pagarigan, i-backupko ti conf folder iti bukodko a proseso ti github kas iti sumaganad

Mangaramid nga umuna iti pribado a bodega

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/ZSCT1t1.webp)

Iserrek ti conf directory ken isubmit iti bodega

```
git init
git add .
git commit -m "init"
git branch -M main
git remote add origin git@github.com:wac-tax-key/conf.git
git push -u origin main
```

## Inayon ti nangipatulod

agtaray

```
chasquid-util user-add i@wac.tax
```

Mabalin nga inayon ti sender

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/khHjLof.webp)

### Beripikar nga umiso ti pannakaisaad ti password

```
chasquid-util authenticate i@wac.tax --password=xxxxxxx
```

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/e92JHXq.webp)

Kalpasan ti pananginayon ti user, ma-update `chasquid/domains/wac.tax/users` , laglagipen nga isubmit daytoy iti bodega.

## DNS add ti rekord ti SPF

Ti SPF ( Sender Policy Framework ) ket maysa a teknolohia ti panangipaneknek ti email a maus-usar a manglapped iti panagkusit iti email.

Beripikarenna ti kinasiasino ti nangipatulod iti koreo babaen ti panangsukimatna no ti IP address ti nangipatulod ket maitunos kadagiti rekord ti DNS ti nagan ti domain nga ibagbagana, a manglapped kadagiti manangallilaw a mangipatulod kadagiti peke nga email.

Ti pananginayon kadagiti rekord ti SPF ket mabalin a manglapped kadagiti email manipud iti pannakailasin a kas spam agingga a mabalin.

No ti domain name server mo ket saan a mangsuporta ti SPF type, inayon laeng ti TXT type record.

Kas pagarigan, ti SPF ti `wac.tax` ket kastoy

`v=spf1 a mx include:_spf.wac.tax include:_spf.google.com ~all`

SPF para `_spf.wac.tax`

`v=spf1 a:smtp.wac.tax ~all`

Panunoten nga addaanak `include:_spf.google.com` ditoy, daytoy ket gapu ta i-configure-ko `i@wac.tax` kas ti mangipatulod nga adres iti Google mailbox inton agangay.

## Konfigurasion ti DNS DMARC

Ti DMARC ket ti abbreviation ti (Domain-based a Panagpabigbig ti Mensahe, Panagreport & Panagtunos).

Daytoy ket maus-usar a mangtiliw kadagiti SPF bounces (mabalin a gapuanan dagiti biddut ti panagisaad, wenno adda sabali nga agpammarang a sika tapno mangipatulod ti spam).

Inayon ti rekord ti TXT `_dmarc` , .

Kastoy ti linaonna

```
v=DMARC1; p=quarantine; fo=1; ruf=mailto:ruf@wac.tax; rua=mailto:rua@wac.tax
```

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/k44P7O3.webp)

Ti kaipapanan ti tunggal parametro ket kastoy

### p (Patakaran) .

Ipamatmatna no kasano a tamingen dagiti email a mapaay ti SPF (Sender Policy Framework) wenno DKIM (DomainKeys Identified Mail) a panangipaneknek. Ti parametro ti p ket mabalin a maikeddeng iti maysa kadagiti tallo a pateg:

* awan: Awan ti maaramid nga aramid, ti laeng resulta ti panangipaneknek ti maisubli iti nangipatulod babaen ti mekanismo ti panagireport iti email.
* Quarantine: Ikabil ti koreo a saan a nakapasa iti beripikasion iti spam folder, ngem saan a direkta nga ilaksid ti koreo.
* reject: Direkta nga ilaksid dagiti email a mapaay iti panangpaneknek.

### fo (Dagiti Pagpilian iti Pannakapaay) .

Ikeddeng ti kaadu ti impormasion nga insubli babaen ti mekanismo ti panagireport. Mabalin a maikeddeng daytoy iti maysa kadagiti sumaganad a pateg:

* 0: Ireport dagiti resulta ti panangipaneknek para kadagiti amin a mensahe
* 1: Ipadamag laeng dagiti mensahe a mapaay iti panangpaneknek
* d: Ipadamag laeng dagiti pannakapaay ti panangipaneknek ti nagan ti domain
* s: ireport laeng dagiti pannakapaay ti panangipaneknek ti SPF
* l: Ipadamag laeng dagiti pannakapaay ti panangipaneknek ti DKIM

### rua & ruf

* rua (Panangireport ti URI para kadagiti Naurnong a report): Ti adres ti email para iti panangawat kadagiti naurnong a report
* ruf (Panagireport ti URI para kadagiti report ti Forensic): email address tapno makaawat kadagiti detalyado a report

## Inayon dagiti rekord ti MX tapno maipatulod dagiti email iti Google Mail

Gapu ta diak nakasarak iti libre a kahon ti korporasion a mangsuporta kadagiti sapasap nga adres (Catch-All, mabalin nga umawat iti aniaman nga email a naipatulod iti daytoy a nagan ti domain, nga awan ti restriksion kadagiti pangrugian), inusarko ti chasquid a mangipatulod iti amin nga email iti Gmail mailbox-ko.

**No addaanka iti bukodmo a bayad a kahon ti koreo ti negosio, pangngaasim ta dika baliwan ti MX ken laksiden daytoy nga addang.**

Urnosen `conf/chasquid/domains/wac.tax/aliases` , ikeddeng ti kahon ti koreo ti panagipatulod

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/OBDl2gw.webp)

`*` ipamatmatna amin nga email, `i` ti email address prefix ti mangipatulod nga agar-aramat a naparsua iti ngato. Tapno maipatulod ti koreo, tunggal agus-usar ket kasapulanna ti mangnayon iti linia.

Kalpasanna inayon ti rekord ti MX (direkta nga itudok ti adres ti baliktad a nagan ti domain ditoy, kas naipakita iti umuna a linia iti pigura iti baba).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/7__KrU8.webp)

Kalpasan a malpas ti konfigurasion, mabalinmo nga usaren dagiti dadduma nga email address tapno mangipatulod kadagiti email iti `i@wac.tax` ken `any123@wac.tax` tapno makitam no makaawatka kadagiti email iti Gmail.

No saan, kitaen ti log ti chasquid ( `grep chasquid /var/log/syslog` ).

## Mangipatulod iti email iti i@wac.tax babaen ti Google Mail

Kalpasan a naawat ti Google Mail ti koreo, natural a ninamnamak a sumungbat iti `i@wac.tax` imbes nga i.wac.tax@gmail.com.

Bisitaen ti [https://mail.google.com/mail/u/1/#settings/accounts](https://mail.google.com/mail/u/1/#settings/accounts) ken i-klik ti "Inayon ti sabali nga email address".

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/PAvyE3C.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/_OgLsPT.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/XIUf6Dc.webp)

Kalpasanna, iserrek ti verification code a naawat ti email a naipatulod.

Kamaudiananna, daytoy ket mabalin a maikeddeng a kas ti default nga adres ti nangipatulod (agraman ti pagpilian a sumungbat babaen ti isu met laeng nga adres).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/a95dO60.webp)

Iti daytoy a wagas, nakompletomi ti pannakaipasdek ti SMTP mail server ken maigiddato iti dayta, usarenmi ti Google Mail a mangipatulod ken umawat kadagiti email.

## Mangipatulod ti test email tapno maammuan no naballigi ti panagisaad

Iserrek ti `ops/chasquid`

Run `direnv allow` to install dependencies (ti direnv ket naikabil iti napalabas a maysa-a-tulbek a proseso ti panangrugi ken ti maysa a kawit ket nainayon iti shell) .

kalpasanna agtaray

```
user=i@wac.tax pass=xxxx to=iuser.link@gmail.com ./sendmail.coffee
```

Ti kaipapanan dagiti parametro ket kastoy

* agar-aramat: SMTP a nagan ti agar-aramat
* pass: password ti SMTP
* iti: umawat

Mabalinmo ti mangipatulod iti test email.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/ae1iWyM.webp)

Mairekomendar ti panagusar ti Gmail tapno makaawat kadagiti pagsubok nga email tapno maammuan no naballigi dagiti panagisaad.

### TLS nga estandarte nga enkripsio

Kas naipakita iti ladawan iti baba, adda daytoy bassit a kandado, a kayatna a sawen a ti sertipiko ti SSL ket sibaballigi a napalubosan.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/SrdbAwh.webp)

Kalpasanna i-click ti "Ipakita ti Orihinal nga Email".

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/qQQsdxg.webp)

### DKIM

Kas naipakita iti ladawan iti baba, ti orihinal a panid ti koreo ti Gmail ket mangiparang ti DKIM, a kayatna a sawen a ti panagisaad ti DKIM ket naballigi.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/an6aXK6.webp)

Sukimaten ti Naawat iti ulo ti orihinal nga email, ket makitam a ti adres ti nangipatulod ket IPV6, a kayatna a sawen a sibaballigi met a naikonfigura ti IPV6.
