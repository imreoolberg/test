# ADIT kasutusjuhend

## Sisukord

Sissejuhatus	
Klassifikaatorid	
Õiguste süsteem	
Join.v1   
UnJoin.v1   
GetJoined.v1   
GetUserInfo.v1   
SetNotifications.v1   
GetNotifications.v1   
SaveDocument.v1  
SaveDocumentFile.v1   
DeflateDocument.v1   
DeleteDocument.v1   
DeleteDocumentFile.v1   
GetDocument.v1   
GetDocument.v2   
GetSendStatus.v1   
GetDocumentFile.v1   
GetDocumentHistory.v1   
GetDocumentList.v1   
PrepareSignature.v1   
ConfirmSignature.v1   
SendDocument.v1   
ShareDocument.v1   
UnShareDocument.v1   
MarkDocumentViewed.v1   
ModifyStatus.v1   
GetUserContacts.v1   

## Muutelugu

| Kuupäev | Versioon | Kirjeldus | Autor |
|-------|----------|----------------|----------------------|
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |

## Sissejuhatus

Antud dokumendi eesmärk on anda ülevaade ADIT-i poolt pakutavatest veebiteenustest. ADIT veebiteenused on mõeldud publitseerimiseks üle X-Tee (ADIT eeldab, et päringus on defineeritud X-Tee päised). Päringute kirjelduse juures on toodud välja päringu parameetrid ning päringu vastuse kuju kahe X-Tee sõnumiprotokolli versiooni jaoks: versioon 2.0 (veebiteenused stiilis _RPC/encoded_) ja versioon 4.0 (veebiteenused stiilis _Document/Literal wrapped_).
Päringu parameetritena on eeldatud alati järgmiste päiste olemasolu:
**X-tee sõnumiprotokoll v2.0:**

| Päis | Selgitus |
| ------ | ------ |
| `<xtee:nimi/>` | päringu nimi. Nt. „ametlikud-dokumendid.join.v1“ |
| `<xtee:id/>` | päringu ID |
| `<xtee:isikukood/>` | päringu teinud isiku kood |
| `<xtee:andmekogu/>` | andmekogu nimi – „ametlikud-dokumendid“ |
| `<xtee:asutus/>` | päringut tegeva asutuse kood |
| `<adit:infosysteem/>` | päringut tegeva infosüsteemi nimi |

**X-tee sõnumiprotokoll v4.0:**
```xml
<xrd:client id:objectType="MEMBER">
	<id:xRoadInstance>tavaliselt riigi ISO kood</id:xRoadInstance>
	<id:memberClass>kliendi tüüp (kas riik,  asutus, ettevõtte, eraisik jne)</id:memberClass>
	<id:memberCode>päringut tegeva asutuse kood</id:memberCode>
    <id:subsystemCode>alamsüsteem, kelle nimel kasutaja päringut teostab</id:subsystemCode>
</xrd:client>
<xrd:service id:objectType="SERVICE">
	<id:xRoadInstance>tavaliselt riigi ISO kood</id:xRoadInstance>
	<id:memberClass>teenuse pakkuja tüüp (kas riik,  asutus, ettevõtte vms)</id:memberClass>
	<id:memberCode>teenuse pakkuja kood</id:memberCode>
	<id:subsystemCode>andmekogu nimi – „adit“</id:subsystemCode>
	<id:serviceCode>päringu nimi (Nt. „join”)</id:serviceCode>
	<id:serviceVersion>päringu versioon (Nt. „v1”)</id:serviceVersion>
</xrd:service>
<xrd:userId>päringu teinud isiku kood</xrd:userId>
<xrd:id>päringu ID</xrd:id>
<xrd:protocolVersion>sõnumiprotokolli versioon (peab olema 4.0)</xrd:protocolVersion>

<adit:infosysteem>päringut tegeva infosüsteemi nimi</adit:infosysteem>
```

Rohkem info X-tee sõnumiprotokolli v4.0 kohta saab tehnilisest spetsifikatsioonist:
http://x-road.eu/docs/x-road_message_protocol_v4.0.pdf

Osade päringute puhul, kus edastatava info maht võib olla väga suur, edastatakse info SOAP manuses oleva XML failina. Sellised failid peavad olema:

1. GZip formaadis kodeeritud
2. Seejärel Base64 kodeeritud

Selliste päringute väljakutsumisel on oluline teada, et kõigepealt tuleb saadud SOAP manus Base64 dekodeerida ning seejärel GZip formaadist lahti pakkida. Vastupidises järjekorras tegevus on nõutav päringute tegemisel saadetavate failide kohta, ehk kõigepealt tuleb veebiteenusele saadetav SOAP manus GZip formaadis kokku pakkida ja seejärel saadud ZIP fail Base64 kodeerida. Kõik päringud, milles kasutatakse manuseid, peavad olema edastatud järgmise „Content-Type“ päisega:

<pre>
Content-Type: multipart/related; type="text/xml" …
</pre>

Päringute vastused, milles sisaldub SOAP manuseid, saadetakse sama „Content-Type“-ga.
SOAP manus edastatakse nii päringu kui vastuse puhul järgmiselt:

<pre>
Content-Type:  text/xml
Content-ID: 601509144816847547373051420339
Content-Transfer-Encoding: base64
Content-Encoding: gzip
…
[manuse_sisu_base64_gzip]
</pre>

Digitaalse allkirjastamise päringute juures tasub tähele panna seda, et allkirja saab anda vaid see isik, kelle nimel päring tehakse. See tähendab, et päringu X-Tee päis ´<xtee:isikukood/>´ peab vastama sellele isikule, kes reaalselt dokumendi allkirjastab, vastasel juhul tagastatakse  veateade.

## Klassifikaatorid

Dokumendi DVK staatused (DOCUMENT_DVK_STATUS). Päringud: [getDocumentList]().

| Väärtus | Selgitus/tähendus |
| ------- | ---------- |
| 100 | Puudub |
| 101 | Ootel |
| 102 | Saatmisel |
| 103 | Saadetud |
| 104 | Katkestatud |
| 105 | Vastu võetud |

Dokumendi toimingud (DOCUMENT_HISTORY_TYPE). Päringud [getDocumentHistory]().

| Väärtus | Selgitus/tähendus |
| ------- | ---------- |
| create | Dokumendi esmane loomine |
| modify | Dokumendi muutmine |
| add_file | Dokumendi faili lisamine |
| modify_file | Dokumendi faili muutmine |
| delete_file | Dokumendi faili kustutamine |
| modify_status | Dokumendi staatuse muutmine |
| send | Dokumendi saatmine |
| share | Dokumendi jagamine |
| lock | Dokumendi lukustamine |
| deflate | Dokumendi arhiveerimine (deflate) |
| sign | Dokumendi digitaalne allkirjastamine |
| delete | Dokumendi kustutamine |
| markViewed | Dokumendi vaadatuks märkimine |

Dokumendi tüübid (DOCUMENT_TYPE). Päringud: [saveDocument](), [getDocument](), [getDocumentList]().

| Väärtus | Selgitus/tähendus |
| ------- | ---------- |
| letter | Kiri |
| application | Avaldus/taotlus |

Faili tüübid (FILE_TYPE). Päringud: [getDocument](), [getDocumentList]().

| Väärtus | Selgitus/tähendus |
| ------- | ---------- |
| document_file | Dokumendi fail |
| signature_container | Digitaalallkirja konteiner (sisaldab dokumendi faile) |
| zip_archive | Dokumendi failidest moodustatud ZIP arhiiv |

Teavituste tüübid (NOTIFICATION_TYPE). Päringud: [setNotifications](), [getNotifications]().

| Väärtus | Selgitus/tähendus |
| ------- | ---------- |
| send | Dokumendi saatmine |
| share | Dokumendi jagamine |
| view | Dokumendi vaatamine |
| modify | Dokumendi muutmine |
| sign | Dokumendi allkirjastamine |

Kasutajate tüübid (USERTYPE). Päringud: [join](), [getJoined]().

| Väärtus | Selgitus/tähendus |
| ------- | ---------- |
| person | Eraisik |
| institution | Institutsioon / asutus |
| company | Ettevõte |

## Õiguste süsteem

ADIT rakendus võimaldab kasutaja ja infosüsteemi põhist õiguste andmist. See tähendab, et kasutaja saab piirata erinevaid infosüsteem enda andmeid muutmast / vaatamast. Selleks on andmebaasis tabel **ACCESS_RESTRICTION**, millel on järgmised väljad:

REMOTE_APPLICATION – viitab infosüsteemile, millele piirang on rakendatud
USER_CODE – kasutaja, kelle puhul antud piirang kehtib
RESTRICTION – piirangu tüüp. Lubatud väärtused on „READ“ / „WRITE“.

Näide: tabelis on järgmine kirje:

|   |   |
| ---- | ---- |
| REMOTE_APPLICATION | KOV |
| USER_CODE | EE38407124998 |
| RESTRICTION | WRITE |

Selline kirje tähendab seda, et infosüsteem „KOV“ ei tohi muuta kasutaja „EE38407124998“ andmeid.

Näide: tabelis on järgmine kirje:

|   |   |
| ---- | ---- |
| REMOTE_APPLICATION | KOV |
| USER_CODE | EE38407124998 |
| RESTRICTION | READ |

Selline kirje tähendab seda, et infosüsteem „KOV“ ei tohi lugeda kasutaja „EE38407124998“ andmeid.

Tabelis **REMOTE_APPLICATION** on kirjeldatud infosüsteemid. Selles tabelis on väljad CAN_READ / CAN_WRITE, mis määravad vastavalt, kas antud infosüsteemil on üleüldine õigus ADIT-is andmeid lugeda / kirjutada. Kui välja väärtuseks on „0“ (või NULL), siis antud infosüsteemil vastav õigus puudub. Kui välja väärtuseks on „1“, siis antud infosüsteemil on vastav õigus.

Päringute tegemisel kontrollitakse esialgu infosüsteemi üldist õigust ADIT-is andmeid lugeda / kirjutada. Kui õigus on olemas, kontrollitakse lisaks ka seda, kas päringu teinud infosüsteemile on kehtestatud antud kasutaja jaoks piiranguid.

## Join.v1

Päring võimaldab liitud ADIT teenusega. Kui kasutaja on juba liitunud, võimaldab sama päring muuta kasutaja nime. Kasutaja identifitseeritakse X-tee päise „isikukood“ järgi. Kui päringu päises määratud isikukoodiga kasutaja juba eksisteerib süsteemis, siis toimib päringuga nimevahetus – kasutajale määratakse päringus näidatud nimi (see osutub vajalikuks juhul kui näiteks asutuse / isiku / firma nimi muutub).
Parameetrid

| Nimi | Väärtuse tüüp | Väärtuse näide |
| --- | --- | --- |
| xtee:isikukood | String | EE30312829989 |
| userType | String | person |
| userName | String | Mati Maamees |

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd" xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid">
   <soapenv:Header>
      <xtee:nimi>ametlikud-dokumendid.join.v1</xtee:nimi>
      <xtee:id>00000000000000</xtee:id>
      <xtee:isikukood>EE30312829989</xtee:isikukood>
      <xtee:andmekogu>ametlikud-dokumendid</xtee:andmekogu>
      <xtee:asutus>12345678</xtee:asutus>
      <adit:infosysteem>KOV</adit:infosysteem>
   </soapenv:Header>
   <soapenv:Body>
      <adit:join>
         <keha>
            <userType>person</userType>
            <userName>Mati Maamees</userName>
         </keha>
      </adit:join>
   </soapenv:Body>
</soapenv:Envelope>
```

**X-tee sõnumiprotokoll v4.0**

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
	xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid"
	xmlns:xrd="http://x-road.eu/xsd/xroad.xsd"
	xmlns:id="http://x-road.eu/xsd/identifiers">
    <soapenv:Header>
        <xrd:client id:objectType="MEMBER">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>BUSINESS</id:memberClass>
            <id:memberCode>12345678</id:memberCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>
			
            <id:subsystemCode>ametlikud-dokumendid</id:subsystemCode>
            <id:serviceCode>join</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>

        <adit:infosysteem>KOV</adit:infosysteem>
    </soapenv:Header>
    <soapenv:Body>
        <adit:join>
            <keha>
                <userType>person</userType>
                <userName>Mati Maamees</userName>
            </keha>
        </adit:join>
    </soapenv:Body>
</soapenv:Envelope>
```
### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**
```xml
<SOAP-ENV:Envelope SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/" xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/" xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd">
   <SOAP-ENV:Header>
      <xtee:asutus xsi:type="xsd:string">12345678</xtee:asutus>
      <xtee:isikukood xsi:type="xsd:string">EE30312829981</xtee:isikukood>
      <xtee:id xsi:type="xsd:string">00000000000000</xtee:id>
      <xtee:nimi xsi:type="xsd:string">ametlikud-dokumendid.join.v1</xtee:nimi>
      <xtee:andmekogu xsi:type="xsd:string">ametlikud-dokumendid</xtee:andmekogu>
   </SOAP-ENV:Header>
   <SOAP-ENV:Body>
      <adit:joinResponse>
         <paring>
            <userType>person</userType>
            <userName>Mati Maamees</userName>
         </paring>
         <keha>
            <success>true</success>
            <messages>
               <message lang="en">User added</message>
               <message lang="et">Kasutaja lisatud</message>
            </messages>
         </keha>
      </adit:joinResponse>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**X-tee sõnumiprotokoll v4.0**
```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"
	xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid"
	xmlns:xrd="http://x-road.eu/xsd/xroad.xsd"
	xmlns:id="http://x-road.eu/xsd/identifiers">
    <SOAP-ENV:Header>
        <xrd:client id:objectType="MEMBER">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>BUSINESS</id:memberClass>
            <id:memberCode>12345678</id:memberCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>	
            <id:subsystemCode>ametlikud-dokumendid</id:subsystemCode>
            <id:serviceCode>join</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <adit:joinResponse>
            <keha>
                <success>true</success>
                <messages>
                    <message lang="en">User added</message>
                    <message lang="et">Kasutaja lisatud</message>
                 </messages>
            </keha>
        </adit:joinResponse>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```
## UnJoin.v1

Päring võimaldab ADIT teenusega liitunud kasutajal oma konto sulgeda / deaktiveerida.

Parameetrid

| Nimi | Väärtuse tüüp | Väärtuse näide |
| --- | --- | --- |
| xtee:isikukood | String | EE30312829989 |

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## GetJoined.v1

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## SetNotifications.v1  

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## GetNotifications.v1   

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## SaveDocument.v1  

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## SaveDocumentFile.v1  

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## DeflateDocument.v1  

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## DeleteDocument.v1 

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## DeleteDocumentFile.v1   

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## GetDocument.v1   

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## GetDocument.v2   

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## GetSendStatus.v1   

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## GetDocumentFile.v1  

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## GetDocumentHistory.v1   

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## GetDocumentList.v1   

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## PrepareSignature.v1   

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## ConfirmSignature.v1   

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## SendDocument.v1   

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## ShareDocument.v1   

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## UnShareDocument.v1   

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## MarkDocumentViewed.v1   

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## ModifyStatus.v1   

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

## GetUserContacts.v1 

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml

```

**X-tee sõnumiprotokoll v4.0**

```xml

```
