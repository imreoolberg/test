# ADIT kasutusjuhend

## Sisukord

- **[Sissejuhatus](#sissejuhatus)**	
   * [Klassifikaatorid](#klassifikaatorid)	
   * [Õiguste süsteem](#Õiguste-süsteem)			
- **[Kasutaja konto](#account)**
  * [Join.v1](#joinv1)   
  * [UnJoin.v1](#unjoinv1)   
  * [GetJoined.v1](#getjoinedv1)  
  * [GetUserInfo.v1](#getuserinfov1)   
  * [GetUserContacts.v1](#getusercontactsv1)   
- [**Teavitused**](#notifications)
  * [SetNotifications.v1](#setnotificationsv1)   
  * [GetNotifications.v1](#getnotificationsv1)   
- [**Dokumentide loomine/muutmine**](#document-modify)
  * [SaveDocument.v1](#savedocumentv1)  
  * [SaveDocumentFile.v1](#savedocumentfilev1)   
  * [DeflateDocument.v1](#deflatedocumentv1)   
  * [DeleteDocument.v1](deletedocumentv1)   
  * [DeleteDocumentFile.v1](deletedocumentfilev1)  
- [**Dokumentide pärimine**](#document-get)
  * GetDocument.v1   
  * GetDocument.v2    
  * GetDocumentFile.v1   
  * GetDocumentHistory.v1   
  * GetDocumentList.v1  
- [**Dokumentide allkirjastamine**](#document-sign)
  * PrepareSignature.v1   
  * ConfirmSignature.v1   
- [**Dokumentide edastus**](#document-deliver)
  * SendDocument.v1   
  * GetSendStatus.v1  
  * ShareDocument.v1   
  * UnShareDocument.v1   
  * MarkDocumentViewed.v1   
  * ModifyStatus.v1  
  
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

<a name="account"></a>
## Kasutaja kontoga seotud päringud

## Join.v1

Päring võimaldab liitud ADIT teenusega. Kui kasutaja on juba liitunud, võimaldab sama päring muuta kasutaja nime. Kasutaja identifitseeritakse X-tee päise „isikukood“ järgi. Kui päringu päises määratud isikukoodiga kasutaja juba eksisteerib süsteemis, siis toimib päringuga nimevahetus – kasutajale määratakse päringus näidatud nimi (see osutub vajalikuks juhul kui näiteks asutuse / isiku / firma nimi muutub).
Parameetrid

| Nimi | Väärtuse tüüp | Väärtuse näide |
| --- | --- | --- |
| xrd:userId  | String | EE30312829989 |
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>			
            <id:subsystemCode>adit</id:subsystemCode>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>	
            <id:subsystemCode>adit</id:subsystemCode>
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
| xrd:userId  | String | EE30312829989 |

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd" xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid">
   <soapenv:Header>
     <xtee:nimi>ametlikud-dokumendid.unJoin.v1</xtee:nimi>
      <xtee:id>00000000000000</xtee:id>
      <xtee:isikukood>EE38407054916</xtee:isikukood>
      <xtee:andmekogu>ametlikud-dokumendid</xtee:andmekogu>
      <xtee:asutus>12345678</xtee:asutus>
      <adit:infosysteem>KOV</adit:infosysteem>
   </soapenv:Header>
   <soapenv:Body>
      <adit:unJoin>
         <keha/>
      </adit:unJoin>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>			
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>unJoin</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>

        <adit:infosysteem>KOV</adit:infosysteem>
    </soapenv:Header>
    <soapenv:Body>
        <adit:unJoin>
            <keha/>
        </adit:unJoin>
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
      <xtee:isikukood xsi:type="xsd:string">EE38407054916</xtee:isikukood>
      <xtee:id xsi:type="xsd:string">00000000000000</xtee:id>
      <xtee:nimi xsi:type="xsd:string">ametlikud-dokumendid.unJoin.v1</xtee:nimi>
      <xtee:andmekogu xsi:type="xsd:string">ametlikud-dokumendid</xtee:andmekogu>
   </SOAP-ENV:Header>
   <SOAP-ENV:Body>
      <adit:unJoinResponse>
         <paring/>
         <keha>
            <success>true</success>
            <messages>
               <message lang="en">User EE38407054916 unjoined</message>
               <message lang="et">User EE38407054916 unjoined</message>
            </messages>
         </keha>
      </adit:unJoinResponse>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>	
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>unJoin</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <adit:unJoinResponse>
            <keha>
                <success>true</success>
                <messages>
                    <message lang="en">User EE38407054916 unjoined</message>
                    <message lang="et">User EE38407054916 unjoined</message>
                </messages>
            </keha>
        </adit:unJoinResponse>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## GetJoined.v1

Päring võimaldab pärida teenusega liitunud kasutajate nimekirja. Parameeter „maxResults“ määrab ära, mitu tulemust maksimaalselt tagastatakse ning parameeter „startIndex“ määrab, millisest leitud tulemuse reast alates tulemused tagastatakse (see võimaldab pärida nö. lehekülgede kaupa). Päringu tulemus tagastatakse SOAP manuses, mis on Base64-s kodeeritud GZip formaadis fail – see tähendab seda, et päringu vastus on vaja kõigepealt Base64 dekodeerida ning seejärel GZip-iga lahti pakkida. Tulemuseks on XML fail.

Parameetrid

| Nimi | Väärtuse tüüp | Väärtuse näide |
| --- | --- | --- |
| maxResults | Positive integer | 2 |
| startIndex | Positive integer | 0 |

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd" xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid">
   <soapenv:Header>
      <xtee:nimi>ametlikud-dokumendid.getJoined.v1</xtee:nimi>
      <xtee:id>00000000000000</xtee:id>
      <xtee:isikukood>EE38407089745</xtee:isikukood>
      <xtee:andmekogu>ametlikud-dokumendid</xtee:andmekogu>
      <xtee:asutus>12345678</xtee:asutus>
      <adit:infosysteem>KOV</adit:infosysteem>
   </soapenv:Header>
   <soapenv:Body>
      <adit:getJoined>
         <keha>
            <maxResults>2</maxResults>
            <startIndex>0</startIndex>
         </keha>
      </adit:getJoined>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>			
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>getJoined</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>

        <adit:infosysteem>KOV</adit:infosysteem>
    </soapenv:Header>
    <soapenv:Body>
        <adit:getJoined>
            <keha>
                <maxResults>2</maxResults>
                <startIndex>0</startIndex>
            </keha>
        </adit:getJoined>
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
      <xtee:isikukood xsi:type="xsd:string">EE38407089745</xtee:isikukood>
      <xtee:id xsi:type="xsd:string">00000000000000</xtee:id>
      <xtee:nimi xsi:type="xsd:string">ametlikud-dokumendid.getJoined.v1</xtee:nimi>
      <xtee:andmekogu xsi:type="xsd:string">ametlikud-dokumendid</xtee:andmekogu>
   </SOAP-ENV:Header>
   <SOAP-ENV:Body>
      <adit:getJoinedResponse>
         <paring>
            <maxResults>2</maxResults>
            <startIndex>0</startIndex>
         </paring>
         <keha>
            <success>true</success>
            <messages>
               <message lang="en">Userlist retrieved for 2 users</message>
               <message lang="et">Userlist retrieved for 2 users</message>
            </messages>
            <user_list href="cid:043990164214936000314841499192" />
         </keha>
      </adit:getJoinedResponse>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>getJoined</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <adit:getJoinedResponse>
            <keha>
                <success>true</success>
                <messages>
                    <message lang="en">Userlist retrieved for 2 users</message>
                    <message lang="et">Userlist retrieved for 2 users</message>
                </messages>
                <user_list href="cid:043990164214936000314841499192" />
            </keha>
        </adit:getJoinedResponse>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**Dekodeeritud SOAP manuse sisu:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<getJoinedResponseAttachmentV1>
    <user_list>
        <total>2</total>
        <user>
            <code>EE40312829989</code>
            <name>Juta Juust</name>
            <type>person</type>
        </user>
        <user>
            <code>EE38407089745</code>
            <name>Kalle Kalamees</name>
            <type>person</type>
        </user>
    </user_list>
</getJoinedResponseAttachmentV1>
```

## GetUserInfo.v1  

Päring võimaldab näha kasutajate detailset informatsiooni. Parameetriga „userList“ on määratud SOAP manuse ID, milles sisaldub kasutajakoodide loend, kelle kohta soovitakse informatsiooni. SOAP manus peab olema GZip-itud XML fail, mis on seejärel Base64 kodeeritud ning lisatud manusena. Päringu vastus tagastatakse samuti SOAP manuses ning samamoodi GZip-ituna ja seejärel Base64 kodeeritult.

Parameetrid

| Nimi | Väärtuse tüüp | Väärtuse näide |
| --- | --- | --- |
| userList | String | cid:123123123 |

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd" xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid">
   <soapenv:Header>
      <xtee:nimi>ametlikud-dokumendid.getUserInfo.v1</xtee:nimi>
      <xtee:id>00000000000000</xtee:id>
      <xtee:isikukood>EE38407089745</xtee:isikukood>
      <xtee:andmekogu>ametlikud-dokumendid</xtee:andmekogu>
      <xtee:asutus>12345678</xtee:asutus>
      <adit:infosysteem>KOV</adit:infosysteem>
   </soapenv:Header>
   <soapenv:Body>
      <adit:getUserInfo>
         <keha>
            <userList href="cid:123123123"/>
         </keha>
      </adit:getUserInfo>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>			
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>getUserInfo</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>

        <adit:infosysteem>KOV</adit:infosysteem>
    </soapenv:Header>
    <soapenv:Body>
        <adit:getUserInfo>
            <keha>
                <userList href="cid:123123123"/>
            </keha>
        </adit:getUserInfo>
    </soapenv:Body>
</soapenv:Envelope>
```

**Päringu SOAP manuse dekodeeritud sisu näide:**

```xml
<?xml version="1.0" encoding="utf-8"?>
<getUserInfoRequestAttachmentV1>
    <user_list>
        <code>EE38407054916</code>
        <code>EE48407089933</code>
        <code>99954321</code>
        <code>EE11111111111</code>
    </user_list>
</getUserInfoRequestAttachmentV1>
```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<SOAP-ENV:Envelope SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/" xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/" xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd">
   <SOAP-ENV:Header>
      <xtee:asutus xsi:type="xsd:string">12345678</xtee:asutus>
      <xtee:isikukood xsi:type="xsd:string">EE38407089745</xtee:isikukood>
      <xtee:id xsi:type="xsd:string">00000000000000</xtee:id>
      <xtee:nimi xsi:type="xsd:string">ametlikud-dokumendid.getUserInfo.v1</xtee:nimi>
      <xtee:andmekogu xsi:type="xsd:string">ametlikud-dokumendid</xtee:andmekogu>
   </SOAP-ENV:Header>
   <SOAP-ENV:Body>
      <adit:getUserInfoResponse>
         <paring>
            <userList>cid:123123123</userList>
         </paring>
         <keha>
            <success>true</success>
            <user_list href="cid:923698868220004834915299871891" />
         </keha>
      </adit:getUserInfoResponse>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>	
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>getUserInfo</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <adit:getUserInfoResponse>
            <keha>
                <success>true</success>
                <user_list href="cid:923698868220004834915299871891" />
            </keha>
        </adit:getUserInfoResponse>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**Dekodeeritud SOAP manuse sisu:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<getUserInfoResponseAttachmentV1>
    <user_list>
        <user>
            <code>EE38407054916</code>
            <name>Eesnimi Perenimi</name>
            <has_joined>true</has_joined>
            <free_space_bytes>10485760</free_space_bytes>
            <used_space_bytes>0</used_space_bytes>
            <can_read>true</can_read>
            <can_write>true</can_write>
            <uses_dvk>false</uses_dvk>
            <messages/>
        </user>
        <user>
            <code>EE48407089933</code>
            <name>Eesnimi Perenimi</name>
            <has_joined>true</has_joined>
            <free_space_bytes>10485760</free_space_bytes>
            <used_space_bytes>0</used_space_bytes>
            <can_read>true</can_read>
            <can_write>true</can_write>
            <uses_dvk>false</uses_dvk>
            <messages/>
        </user>
        <user>
            <code>99954321</code>
            <name>Firmanimi</name>
            <has_joined>true</has_joined>
            <free_space_bytes>10485760</free_space_bytes>
            <used_space_bytes>0</used_space_bytes>
            <can_read>true</can_read>
            <can_write>false</can_write>
            <uses_dvk>true</uses_dvk>
            <messages/>
        </user>
        <user>
            <code>EE11111111111</code>
            <has_joined>false</has_joined>
            <can_read>false</can_read>
            <can_write>false</can_write>
            <uses_dvk>false</uses_dvk>
            <messages>
                <message lang="en">Person/Institution EE11111111111 has not subscribed for ADITs services but they may join the service via http://www.eesti.ee/est/minu/dokumendid</message>
                <message lang="et">Isik/asutus EE11111111111 ei ole teenusega ADIT liitunud, kuid võite soovitada tal teenusega liituda aadressil http://www.eesti.ee/est/minu/dokumendid</message>
            </messages>
        </user>
    </user_list>
</getUserInfoResponseAttachmentV1>
```

## GetUserContacts.v1 

Päring võimaldab pärida isiku suhtluspartnerid. Kontaktid järjestatud sisestamisel esmajärjekorras, ehk viimati kasutatud eespool. Päringu tulemus tagastatakse SOAP manuses, mis on Base64-s kodeeritud GZip formaadis fail – see tähendab seda, et päringu vastus on vaja kõigepealt Base64 dekodeerida ning seejärel GZip-iga lahti pakkida. Tulemuseks on XML fail.

Parameetrid

| Nimi | Väärtuse tüüp | Väärtuse näide |
| --- | --- | --- |
|  xrd:userId | String | EE30312829989 |

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd" xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid">
   <soapenv:Header>
      <xtee:nimi>ametlikud-dokumendid.getUserContacts.v1</xtee:nimi>
      <xtee:id>00000000000000</xtee:id>
      <xtee:isikukood>EE38407089745</xtee:isikukood>
      <xtee:andmekogu>ametlikud-dokumendid</xtee:andmekogu>
      <xtee:asutus>12345678</xtee:asutus>
      <adit:infosysteem>KOV</adit:infosysteem>
   </soapenv:Header>
   <soapenv:Body>
      <adit:getUserContacts>
         <keha/>
      </adit:getUserContacts>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>			
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>getUserContacts</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>

        <adit:infosysteem>KOV</adit:infosysteem>
    </soapenv:Header>
    <soapenv:Body>
        <adit:getUserContacts>
            <keha/>
        </adit:getUserContacts>
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
      <xtee:isikukood xsi:type="xsd:string">EE38407089745</xtee:isikukood>
      <xtee:id xsi:type="xsd:string">00000000000000</xtee:id>
      <xtee:nimi xsi:type="xsd:string">ametlikud-dokumendid.getUserContacts.v1</xtee:nimi>
      <xtee:andmekogu xsi:type="xsd:string">ametlikud-dokumendid</xtee:andmekogu>
   </SOAP-ENV:Header>
   <SOAP-ENV:Body>
      <adit:getUserContactsResponse>
         <paring/>
         <keha>
            <adit:success>true</adit:success>
            <adit:messages>
               <message lang="et">Kasutaja kontaktide nimekiri väljastatud: 2 kontakti.</message>
               <message lang="en">User contact list retrieved for 2 users.</message>
               <message lang="ru">Kasutaja kontaktide nimekiri väljastatud: 2 kontakti.</message>
            </adit:messages>
             <userContactList href="cid:877866174409502118726932381503"/>
         </keha>
      </adit:getUserContactsResponse>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>	
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>getUserContacts</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <adit:getUserContactsResponse>
            <keha>
                <adit:success>true</adit:success>
                <adit:messages>
                    <message lang="et">Kasutaja kontaktide nimekiri väljastatud: 2 kontakti.</message>
                    <message lang="en">User contact list retrieved for 2 users.</message>
                    <message lang="ru">Kasutaja kontaktide nimekiri väljastatud: 2 kontakti.</message>
                </adit:messages>
                <userContactList href="cid:877866174409502118726932381503"/>
            </keha>
        </adit:getUserContactsResponse>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**Dekodeeritud SOAP manuse sisu:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<user_contact_list>
    <total>2</total>
    <user_contact>
        <code>EE40312829989</code>
        <name>Juta Juust</name>
        <type>person</type>
    </user_contact>
    <user_contact>
        <code>EE38407089745</code>
        <name>Kalle Kalamees</name>
        <type>person</type>
    </user_contact>
</user_contact_list>
```

<a name="notifications"></a>
## Teavitused

## SetNotifications.v1  

Päring võimaldab seadistada kasutaja teavituskalendrisse saadetavad teavitused. Parameetriga „type“ määratakse teavituse tüüp. Parameetriga „active“ määratakse, kas antud teavituse tüüp on aktiveeritud (määrates siin väärtuseks „false“ on võimalik seda tüüpi teavitus välja lülitada).

Parameetrid

| Nimi | Väärtuse tüüp | Väärtuse näide |
| --- | --- | --- |
| xrd:userId | String | EE30312829989 |
| type | String | send |
| active | Boolean | true |

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd" xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid">
   <soapenv:Header>
      <xtee:nimi>ametlikud-dokumendid.setNotifications.v1</xtee:nimi>
      <xtee:id>00000000000000</xtee:id>
      <xtee:isikukood>EE38407089745</xtee:isikukood>
      <xtee:andmekogu>ametlikud-dokumendid</xtee:andmekogu>
      <xtee:asutus>12345678</xtee:asutus>
      <adit:infosysteem>KOV</adit:infosysteem>
   </soapenv:Header>
   <soapenv:Body>
      <adit:setNotifications>
         <keha>
            <notifications>
               <notification>
                  <type>send</type>
                  <active>true</active>
               </notification>
               <notification>
                  <type>share</type>
                  <active>false</active>
               </notification>
            </notifications>
         </keha>
      </adit:setNotifications>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>			
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>setNotifications</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>

        <adit:infosysteem>KOV</adit:infosysteem>
    </soapenv:Header>
    <soapenv:Body>
        <adit:setNotifications>
            <keha>
                <notifications>
                    <notification>
                        <type>send</type>
                        <active>true</active>
                    </notification>
                    <notification>
                        <type>share</type>
                        <active>false</active>
                    </notification>
                </notifications>
            </keha>
        </adit:setNotifications>
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
      <xtee:isikukood xsi:type="xsd:string">EE38407089745</xtee:isikukood>
      <xtee:id xsi:type="xsd:string">00000000000000</xtee:id>
      <xtee:nimi xsi:type="xsd:string">ametlikud-dokumendid.setNotifications.v1</xtee:nimi>
      <xtee:andmekogu xsi:type="xsd:string">ametlikud-dokumendid</xtee:andmekogu>
   </SOAP-ENV:Header>
   <SOAP-ENV:Body>
      <adit:setNotificationsResponse>
         <paring>
            <notifications>
               <notification>
                  <type>send</type>
                  <active>true</active>
               </notification>
               <notification>
                  <type>share</type>
                  <active>false</active>
               </notification>
            </notifications>
         </paring>
         <keha>
            <success>true</success>
            <messages>
               <message lang="en">User notifications were successfully modified.</message>
               <message lang="et">Kasutaja teavituste seadistus muudetud.</message>
            </messages>
         </keha>
      </adit:setNotificationsResponse>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>setNotifications</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <adit:setNotificationsResponse>
            <keha>
                <success>true</success>
                <messages>
                    <message lang="en">User notifications were successfully modified.</message>
                    <message lang="et">Kasutaja teavituste seadistus muudetud.</message>
                </messages>
            </keha>
        </adit:setNotificationsResponse>
   </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## GetNotifications.v1   

Päring võimaldab pärida kasutaja teavituste seadistuse andmed. Päringu kehas parameetrid puuduvad, oluline on X-tee päise „isikukood“ olemasolu, mille järgi tuvastatakse kasutaja, kelle andmeid päritakse. Päring on mõeldud kasutajale enda seadistuse pärimiseks. Päringu vastuses on toodud teavituste tüübid ning näidatud kas antud liiki teavitus on aktiveeritud.

Parameetrid

| Nimi | Väärtuse tüüp | Väärtuse näide |
| --- | --- | --- |
| xrd:userId | String | EE30312829989 |

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd" xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid">
   <soapenv:Header>
      <xtee:nimi>ametlikud-dokumendid.getNotifications.v1</xtee:nimi>
      <xtee:id>00000000000000</xtee:id>
      <xtee:isikukood>EE38407089745</xtee:isikukood>
      <xtee:andmekogu>ametlikud-dokumendid</xtee:andmekogu>
      <xtee:asutus>12345678</xtee:asutus>
      <adit:infosysteem>KOV</adit:infosysteem>
   </soapenv:Header>
   <soapenv:Body>
      <adit:getNotifications>
         <keha/>
      </adit:getNotifications>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>			
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>getNotifications</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>

        <adit:infosysteem>KOV</adit:infosysteem>
    </soapenv:Header>
    <soapenv:Body>
        <adit:getNotifications>
            <keha/>
        </adit:getNotifications>
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
      <xtee:isikukood xsi:type="xsd:string">EE38407089745</xtee:isikukood>
      <xtee:id xsi:type="xsd:string">00000000000000</xtee:id>
      <xtee:nimi xsi:type="xsd:string">ametlikud-dokumendid.getNotifications.v1</xtee:nimi>
      <xtee:andmekogu xsi:type="xsd:string">ametlikud-dokumendid</xtee:andmekogu>
   </SOAP-ENV:Header>
   <SOAP-ENV:Body>
      <adit:getNotificationsResponse>
         <paring/>
         <keha>
            <success>true</success>
            <messages>
               <message lang="en">User notifications were successfully retrieved.</message>
            </messages>
            <notifications>
               <notification>
                  <type>send</type>
                  <active>true</active>
               </notification>
               <notification>
                  <type>share</type>
                  <active>false</active>
               </notification>
               <notification>
                  <type>view</type>
                  <active>false</active>
               </notification>
               <notification>
                  <type>modify</type>
                  <active>false</active>
               </notification>
               <notification>
                  <type>sign</type>
                  <active>false</active>
               </notification>
            </notifications>
         </keha>
      </adit:getNotificationsResponse>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>	
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>getNotifications</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <adit:getNotificationsResponse>
            <keha>
                <success>true</success>
                <messages>
                    <message lang="en">User notifications were successfully retrieved.</message>
                </messages>
                <notifications>
                    <notification>
                        <type>send</type>
                        <active>true</active>
                    </notification>
                    <notification>
                        <type>share</type>
                        <active>false</active>
                    </notification>
                    <notification>
                        <type>view</type>
                        <active>false</active>
                    </notification>
                    <notification>
                        <type>modify</type>
                        <active>false</active>
                    </notification>
                    <notification>
                        <type>sign</type>
                        <active>false</active>
                    </notification>
                </notifications>
            </keha>
        </adit:getNotificationsResponse>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```


<a name="document-modify"></a>
## Dokumentide loomine, muutmine, kustutamine

## SaveDocument.v1  

Päring võimaldab salvestada dokumendi ADIT-is. Dokument lisatakse kasutaja kontole. Päringu parameetri „document“ atribuut „href“ viitab SOAP manusele, milles on salvestatava dokumendi andmed. Päringu vastuses tagastatakse salvestatud dokumendi ID parameetriga „document_id“.
Kui saveDocument päringuga lisatakse uut dokumenti ning dokumendi ainsaks failiks on DigiDoc konteiner, siis teostab ADIT DigiDoc konteineri lahtipakkimise (st lisab eraldi failidena ka DigiDoc konteineris sisaldunud failid ning loeb välja allkirjade andmed).

Parameetrid

| Nimi | Väärtuse tüüp | Väärtuse näide |
| --- | --- | --- |
| xrd:userId | String | EE30312829989 |
| Document href | String | cid:123123123 |

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd" xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid">
   <soapenv:Header>
     <xtee:nimi>ametlikud-dokumendid.saveDocument.v1</xtee:nimi>
      <xtee:id>00000000000000</xtee:id>
      <xtee:isikukood>EE38407054916</xtee:isikukood>
      <xtee:andmekogu>ametlikud-dokumendid</xtee:andmekogu>
      <xtee:asutus>12345678</xtee:asutus>
      <adit:infosysteem>KOV</adit:infosysteem>
   </soapenv:Header>
   <soapenv:Body>
      <adit:saveDocument>
        <keha>
          <document href="cid:123123123"/>
          </keha>
      </adit:saveDocument>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>			
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>saveDocument</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>

        <adit:infosysteem>KOV</adit:infosysteem>
    </soapenv:Header>
    <soapenv:Body>
        <adit:saveDocument>
            <keha>
                <document href="cid:123123123"/>
            </keha>
        </adit:saveDocument>
    </soapenv:Body>
</soapenv:Envelope>
```

**Päringu SOAP manuse parameetrid**

| Nimi | Kirjeldus | Väärtuse tüüp | Väärtuse näide |
| --- | --- | --- | --- |
| Document | Dokumendi andmete konteiner |  |  |
| id | Dokumendi ID (tühi, kui salvestatakse uut dokumenti, täidetud kui muudetakse olemasolevat dokumenti) | Integer | 45 |
| guid | Dokumendi GUID | String | 19d71714-0d93-292a-b254-3a06a4f24002 |
| title | Dokumendi pealkiri | String |  |
| document_type | Dokumendi tüüp (application / letter) | String | Application |
| previous_document_id |  | Integer | 33 |
| eform_use_id | Evormi kasutuselevõtu id. Tidetakse ainult siis, kui tegemist on evormi andmetega | Integer | 299 |
| content | Dokumendi sisu. Riigiportaali „minu postkast“ kasutab seda välja nagu emaili keha. | String | Lorem ipsum dolor sit amet |
| files | Dokumendi failide konteiner |  |  |
| file | Dokumendi faili andmed |  |  |
| file.id | Faili id (määratud juhul kui soovitakse muuta olemasolevat dokumendi faili) | Integer | 56 |
| file.name | Faili nimi koos laiendiga | String | Avaldus.txt |
| file.content_type | Faili MIME tüüp | String | Application/pdf |
| file.description | Faili pealkiri või kirjeldus | String | Avaldus Tallinna Linnavalitsusele |
| file.size_bytes | Faili suurus baitides | Long | 300 |
| file.data | Faili sisu Base64 kodeeringus | String | SvVnZXZhI… |

**Päringu SOAP manuse dekodeeriutd sisu näide**

```xml
<?xml version="1.0" encoding="utf-8"?>
<saveDocumentRequestAttachmentV1>
  <document>
    <id/>
    <guid>19d71714-0d93-292a-b254-3a06a4f24002</guid>
    <title>Avaldus Jõgeva Linnavalitsusele</title>
    <document_type>application</document_type>
    <previous_document_id/>
    <eform_use_id>299</eform_use_id>
    <content>Lorem ipsum dolor sit amet</content>
    <files>
      <file>
        <id/>
        <name>avaldus1.txt</name>
        <content_type>text/plain</content_type>
        <description>Avaldus tekstifailina</description>
        <size_bytes>300</size_bytes>
        <data>
        SvVnZXZhIExpbm5hdmFsaXRzdXMJMjAuMDQuMjAxMA0KDQpBdmFsZHVzDQoNClBh
        bHVuIHZpaXZpdGFtYXR1bHQgdGFnYXN0YWRhIG1pbnUgcG9vbHQgbfb2ZHVudWQg
        dGVpc2lw5GV2YWwgbGlubmF2YWxpdHN1c2Uga2FudHNlbGVpc3NlIHVudXN0YXR1
        ZCBrb2h2ZXIsIHN1dXNhZCBqYSByYW5uYXBhbGwuDQoNCg0KRWlubyBNdWlkdWdp
        DQpBUyBBc3V0dXMNCkp1aGF0dXNlIGVzaW1lZXMNCg==
        </data>
      </file>
      <file>
        <id/>
        <name>avaldus2.txt</name>
        <content_type>text/plain</content_type>
        <description>Avaldus tekstifailina</description>
        <size_bytes>300</size_bytes>
        <data>
        SvVnZXZhIExpbm5hdmFsaXRzdXMJMjAuMDQuMjAxMA0KDQpBdmFsZHVzDQoNClBh
        bHVuIHZpaXZpdGFtYXR1bHQgdGFnYXN0YWRhIG1pbnUgcG9vbHQgbfb2ZHVudWQg
        dGVpc2lw5GV2YWwgbGlubmF2YWxpdHN1c2Uga2FudHNlbGVpc3NlIHVudXN0YXR1
        ZCBrb2h2ZXIsIHN1dXNhZCBqYSByYW5uYXBhbGwuDQoNCg0KRWlubyBNdWlkdWdp
        DQpBUyBBc3V0dXMNCkp1aGF0dXNlIGVzaW1lZXMNCg==
        </data>
      </file>
      <file>
        <id/>
        <name>avaldus3.txt</name>
        <content_type>text/plain</content_type>
        <description>Avaldus tekstifailina</description>
        <size_bytes>300</size_bytes>
        <data>
        SvVnZXZhIExpbm5hdmFsaXRzdXMJMjAuMDQuMjAxMA0KDQpBdmFsZHVzDQoNClBh
        bHVuIHZpaXZpdGFtYXR1bHQgdGFnYXN0YWRhIG1pbnUgcG9vbHQgbfb2ZHVudWQg
        dGVpc2lw5GV2YWwgbGlubmF2YWxpdHN1c2Uga2FudHNlbGVpc3NlIHVudXN0YXR1
        ZCBrb2h2ZXIsIHN1dXNhZCBqYSByYW5uYXBhbGwuDQoNCg0KRWlubyBNdWlkdWdp
        DQpBUyBBc3V0dXMNCkp1aGF0dXNlIGVzaW1lZXMNCg==
        </data>
      </file>
    </files>
  </document>
</saveDocumentRequestAttachmentV1>
```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<SOAP-ENV:Envelope SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/" xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/" xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd">
   <SOAP-ENV:Header>
      <xtee:asutus xsi:type="xsd:string">EE12345678</xtee:asutus>
      <xtee:isikukood xsi:type="xsd:string">EE38407089745</xtee:isikukood>
      <xtee:id xsi:type="xsd:string">00000000000000</xtee:id>
      <xtee:nimi xsi:type="xsd:string">ametlikud-dokumendid.saveDocument.v1</xtee:nimi>
      <xtee:andmekogu xsi:type="xsd:string">ametlikud-dokumendid</xtee:andmekogu>
   </SOAP-ENV:Header>
   <SOAP-ENV:Body>
      <adit:saveDocumentResponse>
         <paring>
            <document href="cid:123123123"/>
         </paring>
         <keha>
            <success>true</success>
            <messages>
               <message lang="en">Document was successfully saved.</message>
            </messages>
            <document_id>45</document_id>
         </keha>
      </adit:saveDocumentResponse>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>	
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>saveDocument</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <adit:saveDocumentResponse>
            <keha>
                <success>true</success>
                <messages>
                    <message lang="en">Document was successfully saved.</message>
                </messages>
                <document_id>45</document_id>
            </keha>
        </adit:saveDocumentResponse>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## SaveDocumentFile.v1  

Päring võimaldab salvestata olemasoleva dokumendi juurde faili. Päringu manuses on lisatava faili andmed. Päringu vastusega tagastatakse lisatud faili ID.
Kui saveDocumentFile päringuga lisatakse dokumendile esimest faili ning tegemist on DigiDoc konteineriga, siis teostab ADIT DigiDoc konteineri lahtipakkimise (st lisab eraldi failidena ka DigiDoc konteineris sisaldunud failid ning loeb välja allkirjade andmed).

Parameetrid

| Nimi | Väärtuse tüüp | Väärtuse näide |
| --- | --- | --- |
| xrd:userId | String | EE30312829989 |
| Document_id | Integer | 22 |
| File href | String | cid:123123123 |

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd" xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid">
   <soapenv:Header>
     <xtee:nimi>ametlikud-dokumendid.saveDocumentFile.v1</xtee:nimi>
      <xtee:id>00000000000000</xtee:id>
      <xtee:isikukood>EE38407089745</xtee:isikukood>
      <xtee:andmekogu>ametlikud-dokumendid</xtee:andmekogu>
      <xtee:asutus>EE12345678</xtee:asutus>
      <adit:infosysteem>KOV</adit:infosysteem>
   </soapenv:Header>
   <soapenv:Body>
      <adit:saveDocumentFile>
         <keha>
            <document_id>45</document_id>
            <file href="cid:123123123"/>
         </keha>
      </adit:saveDocumentFile>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>			
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>saveDocumentFile</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>

        <adit:infosysteem>KOV</adit:infosysteem>
    </soapenv:Header>
    <soapenv:Body>
        <adit:saveDocumentFile>
            <keha>
                <document_id>45</document_id>
                <file href="cid:123123123"/>
            </keha>
        </adit:saveDocumentFile>
    </soapenv:Body>
</soapenv:Envelope>
```

**Päringu SOAP manuse parameetrid**

| Nimi | Kirjeldus | Tüüp | Väärtuse näide |
| --- | --- | --- | --- |
| file | Dokumendi faili andmed |  |  |
| file.id | Faili id (määratud juhul kui soovitakse muuta olemasolevat dokumendi faili) | Integer | 56 |
| file.name | Faili nimi koos laiendiga | String | Avaldus.txt |
| file.content_type | Faili MIME tüüp | String | Application/pdf |
| file.description | Faili pealkiri või kirjeldus | String | Avaldus Tallinna Linnavalitsusele |
| file.size_bytes | Faili suurus baitides | Long | 300 |
| file.data | Faili sisu Base64 kodeeringus | String | SvVnZXZhI… |

**Päringu SOAP manuse dekodeeritud sisu näide:**

```xml
<?xml version="1.0" encoding="utf-8"?>
<saveDocumentFileAttachmentV1>
    <file>
        <id/>
        <name>File1.txt</name>
        <content_type>text/plain</content_type>
        <description>SaveDocumentFile insert osa testimine</description>
        <data>
            Q291cmFnZSBpcyByZXNpc3RhbmNlIHRvIGZlYXIsIG1hc3Rlcnkgb2YgZmVhciAt
            IG5vdCBhYnNlbmNlIG9mIGZlYXIuDQotLSBNYXJrIFR3YWluDQoNCkRvbid0IGdv
            IGFyb3VuZCBzYXlpbmcgdGhlIHdvcmxkIG93ZXMgeW91IGEgbGl2aW5nLiBUaGUg
            d29ybGQgb3dlcyB5b3Ugbm90aGluZy4gSXQgd2FzIGhlcmUgZmlyc3QuDQotLSBN
            YXJrIFR3YWluDQo=
        </data>
    </file>
</saveDocumentFileAttachmentV1>
```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<SOAP-ENV:Envelope SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/" xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/" xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd">
   <SOAP-ENV:Header>
      <xtee:asutus xsi:type="xsd:string">EE12345678</xtee:asutus>
      <xtee:isikukood xsi:type="xsd:string">EE38407089745</xtee:isikukood>
      <xtee:id xsi:type="xsd:string">00000000000000</xtee:id>
      <xtee:nimi xsi:type="xsd:string">ametlikud-dokumendid.saveDocumentFile.v1</xtee:nimi>
      <xtee:andmekogu xsi:type="xsd:string">ametlikud-dokumendid</xtee:andmekogu>
   </SOAP-ENV:Header>
   <SOAP-ENV:Body>
      <adit:saveDocumentFileResponse>
         <paring>
            <document_id>45</document_id>
            <file href="cid:123123123"/>
         </paring>
         <keha>
            <success>true</success>
            <messages>
               <message lang="en">Document file was successfully saved.</message>
            </messages>
            <file_id>14</file_id>
         </keha>
      </adit:saveDocumentFileResponse>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>saveDocumentFile</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <adit:saveDocumentFileResponse>
            <keha>
                <success>true</success>
                <messages>
                    <message lang="en">Document file was successfully saved.</message>
                </messages>
                <file_id>14</file_id>
            </keha>
        </adit:saveDocumentFileResponse>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## DeflateDocument.v1  

Päring võimaldab tühjendada dokumendi failid – failide sisu kustutatakse ning asendatakse räsiga. See võimaldab hoida kokku kasutaja andmemahu limiiti. Päringu parameetriga „document_id“ määratakse ära, millise dokumendi faile tahetakse tühjendada.

Parameetrid

| Nimi | Väärtuse tüüp | Väärtuse näide |
| --- | --- | --- |
| Document_id | Integer | 44 |

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd" xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid">
   <soapenv:Header>
      <xtee:nimi>ametlikud-dokumendid.deflateDocument.v1</xtee:nimi>
      <xtee:id>00000000000000</xtee:id>
      <xtee:isikukood>EE38407089745</xtee:isikukood>
      <xtee:andmekogu>ametlikud-dokumendid</xtee:andmekogu>
      <xtee:asutus>12345678</xtee:asutus>
      <adit:infosysteem>KOV</adit:infosysteem>
   </soapenv:Header>
   <soapenv:Body>
      <adit:deflateDocument>
         <keha>
            <document_id>44</document_id>
         </keha>
      </adit:deflateDocument>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>			
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>deflateDocument</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>

        <adit:infosysteem>KOV</adit:infosysteem>
    </soapenv:Header>
    <soapenv:Body>
        <adit:deflateDocument>
            <keha>
                <document_id>44</document_id>
            </keha>
        </adit:deflateDocument>
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
      <xtee:isikukood xsi:type="xsd:string">EE38407089745</xtee:isikukood>
      <xtee:id xsi:type="xsd:string">00000000000000</xtee:id>
      <xtee:nimi xsi:type="xsd:string">ametlikud-dokumendid.deflateDocument.v1</xtee:nimi>
      <xtee:andmekogu xsi:type="xsd:string">ametlikud-dokumendid</xtee:andmekogu>
   </SOAP-ENV:Header>
   <SOAP-ENV:Body>
      <adit:deflateDocumentResponse>
         <paring>
            <document_id>44</document_id>
         </paring>
         <keha>
            <success>true</success>
            <messages>
               <message lang="en">Document was successfully deflated</message>
            </messages>
         </keha>
      </adit:DeflateDocumentResponse>
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
            <id:serviceCode>deflateDocument</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <adit:deflateDocumentResponse>
            <keha>
                <success>true</success>
                <messages>
                    <message lang="en">Document was successfully deflated</message>
                </messages>
            </keha>
        </adit:DeflateDocumentResponse>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## DeleteDocument.v1 

Päring võimaldab kustutada dokumendi – märkida dokumendi kustutatuks. Dokumendi failide sisu kustutatakse ning asendatakse räsiga.

Parameetrid

| Nimi | Väärtuse tüüp | Väärtuse näide |
| --- | --- | --- |
| Document_id | Integer | 44 |

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd" xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid">
   <soapenv:Header>
      <xtee:nimi>ametlikud-dokumendid.deleteDocument.v1</xtee:nimi>
      <xtee:id>00000000000000</xtee:id>
      <xtee:isikukood>EE38407089745</xtee:isikukood>
      <xtee:andmekogu>ametlikud-dokumendid</xtee:andmekogu>
      <xtee:asutus>12345678</xtee:asutus>
      <adit:infosysteem>KOV</adit:infosysteem>
   </soapenv:Header>
   <soapenv:Body>
      <adit:deleteDocument>
         <keha>
            <document_id>45</document_id>
         </keha>
      </adit:deleteDocument>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>			
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>deleteDocument</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>

        <adit:infosysteem>KOV</adit:infosysteem>
    </soapenv:Header>
    <soapenv:Body>
        <adit:deleteDocument>
            <keha>
                <document_id>45</document_id>
            </keha>
        </adit:deleteDocument>
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
      <xtee:isikukood xsi:type="xsd:string">EE38407089745</xtee:isikukood>
      <xtee:id xsi:type="xsd:string">00000000000000</xtee:id>
      <xtee:nimi xsi:type="xsd:string">ametlikud-dokumendid.deleteDocument.v1</xtee:nimi>
      <xtee:andmekogu xsi:type="xsd:string">ametlikud-dokumendid</xtee:andmekogu>
   </SOAP-ENV:Header>
   <SOAP-ENV:Body>
      <adit:deleteDocumentResponse>
         <paring>
            <document_id>45</document_id>
         </paring>
         <keha>
            <success>true</success>
            <messages>
               <message lang="en">Document was successfully deleted.</message>
            </messages>
         </keha>
      </adit:deleteDocumentResponse>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>deleteDocument</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <adit:deleteDocumentResponse>
            <keha>
                <success>true</success>
                <messages>
                    <message lang="en">Document was successfully deleted.</message>
                </messages>
            </keha>
        </adit:deleteDocumentResponse>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## DeleteDocumentFile.v1   

Päring võimaldab kustutada dokumendi juurde kuuluva faili. Fail märgitakse kustutatuks ning faili sisu asendatakse räsiga.

Parameetrid

| Nimi | Väärtuse tüüp | Väärtuse näide |
| --- | --- | --- |
| Document_id | Integer | 44 |
| File_id | Integer | 15 |

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd" xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid">
   <soapenv:Header>
      <xtee:nimi>ametlikud-dokumendid.deleteDocumentFile.v1</xtee:nimi>
      <xtee:id>00000000000000</xtee:id>
      <xtee:isikukood>EE38407089745</xtee:isikukood>
      <xtee:andmekogu>ametlikud-dokumendid</xtee:andmekogu>
      <xtee:asutus>12345678</xtee:asutus>
      <adit:infosysteem>KOV</adit:infosysteem>
   </soapenv:Header>
   <soapenv:Body>
      <adit:deleteDocumentFile>
         <keha>
            <document_id>46</document_id>
            <file_id>15</file_id>
         </keha>
      </adit:deleteDocumentFile>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>			
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>deleteDocumentFile</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>
        <adit:infosysteem>KOV</adit:infosysteem>
    </soapenv:Header>
    <soapenv:Body>
        <adit:deleteDocumentFile>
            <keha>
                <document_id>46</document_id>
                <file_id>15</file_id>
            </keha>
        </adit:deleteDocumentFile>
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
      <xtee:isikukood xsi:type="xsd:string">EE38407089745</xtee:isikukood>
      <xtee:id xsi:type="xsd:string">00000000000000</xtee:id>
      <xtee:nimi xsi:type="xsd:string">ametlikud-dokumendid.deleteDocumentFile.v1</xtee:nimi>
      <xtee:andmekogu xsi:type="xsd:string">ametlikud-dokumendid</xtee:andmekogu>
   </SOAP-ENV:Header>
   <SOAP-ENV:Body>
      <adit:deleteDocumentFileResponse>
         <paring>
            <document_id>46</document_id>
            <file_id>15</file_id>
         </paring>
         <keha>
            <success>true</success>
            <messages>
               <message lang="en">File was successfully deleted</message>
            </messages>
         </keha>
      </adit:deleteDocumentFileResponse>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>	
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>deleteDocumentFile</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <adit:deleteDocumentFileResponse>
            <keha>
                <success>true</success>
                <messages>
                    <message lang="en">File was successfully deleted</message>
                </messages>
            </keha>
        </adit:deleteDocumentFileResponse>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```


<a name="document-get"></a>
## Dokumentide pärimine

## GetDocument.v1   

Päring võimaldab pärida dokumendi andmed. Dokumenti on võimalik pärida ka koos failidega (päringu vastuses tagastatakse ka dokumendi juurde kuuluvate failide sisu) – selleks tuleb määrata parameetri „include_file_contents“ väärtuseks „true“.

Parameetriga „file_types“ saab piirata, millist tüüpi faile soovitakse alla laadida. Võimalikud väärtused:
   - document_file  =  dokumendi failid
   - signature_container  =  digitaalallkirja konteiner
   - zip_archive  =  dokumendi failidest moodustatud ZIP arhiiv
Tüüpi „zip_archive“ saab kasutada ainult juhul, kui „include_file_contents“ väärtuseks on „true“.

Dokumendi andmed tagastatakse SOAP manusena. Viide SOAP manusele lisatakse vastuse parameetri „document“ atribuuti „href“.

Parameetrid

| Nimi | Väärtuse tüüp | Väärtuse näide |
| --- | --- | --- |
| Document_id | Integer | 46 |
| Include_file_contents | Boolean | true |
| file_types | Faili tüüpide massiiv |  |
| file_types.file_type | Faili tüübi nimetus | signature_container |

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd" xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid">
   <soapenv:Header>
      <xtee:nimi>ametlikud-dokumendid.getDocument.v1</xtee:nimi>
      <xtee:id>00000000000000</xtee:id>
      <xtee:isikukood>EE38407089745</xtee:isikukood>
      <xtee:andmekogu>ametlikud-dokumendid</xtee:andmekogu>
      <xtee:asutus>12345678</xtee:asutus>
      <adit:infosysteem>KOV</adit:infosysteem>
   </soapenv:Header>
   <soapenv:Body>
      <adit:getDocument>
         <keha>
            <document_id>46</document_id>
            <include_file_contents>true</include_file_contents>
            <file_types>
               <file_type>document_file</file_type>
               <file_type>signature_container</file_type>
            </file_types>
         </keha>
      </adit:getDocument>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>			
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>getDocument</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>

        <adit:infosysteem>KOV</adit:infosysteem>
    </soapenv:Header>
    <soapenv:Body>
        <adit:getDocument>
            <keha>
                <document_id>46</document_id>
                <include_file_contents>true</include_file_contents>
                <file_types>
                    <file_type>document_file</file_type>
                    <file_type>signature_container</file_type>
                </file_types>
            </keha>
        </adit:getDocument>
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
      <xtee:isikukood xsi:type="xsd:string">EE38407089745</xtee:isikukood>
      <xtee:id xsi:type="xsd:string">00000000000000</xtee:id>
      <xtee:nimi xsi:type="xsd:string">ametlikud-dokumendid.getDocument.v1</xtee:nimi>
      <xtee:andmekogu xsi:type="xsd:string">ametlikud-dokumendid</xtee:andmekogu>
   </SOAP-ENV:Header>
   <SOAP-ENV:Body>
      <adit:getDocumentResponse>
         <paring>
            <document_id>46</document_id>
            <include_file_contents>true</include_file_contents>
            <file_types>
               <file_type>document_file</file_type>
               <file_type>signature_container</file_type>
            </file_types>
         </paring>
         <keha>
            <success>true</success>
            <messages>
               <message lang="en">Requested document was successfully retrieved</message>
            </messages>
            <document href="cid:288593275620550871934750578520" />
          </keha>
      </adit:getDocumentResponse>
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
	    <id:subsystemCode>generic-consumer</id:subsystemCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>getDocument</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <adit:getDocumentResponse>
            <keha>
                <success>true</success>
                <messages>
                    <message lang="en">Requested document was successfully retrieved</message>
                </messages>
                <document href="cid:288593275620550871934750578520" />
            </keha>
        </adit:getDocumentResponse>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**Päringu vastuse SOAP manuse dekodeeritud sisu parameetrid:**

| Nimi | Kirjeldus | Tüüp | Väärtuse näide |
| --- | --- | --- | --- |
| Document | Dokumendi andmete konteiner |  |  |
| id | Dokumendi ID | Integer | 45 |
| guid | Dokumendi GUID | String | 19d71714-0d93-292a-b254-3a06a4f24002 |
| title | Dokumendi pealkiri | String |  |
| content | Dokumendi sisu | String | Lorem ipsum sit amet |
| folder | Dokumendi kaust. Võimalikud väärtused: local, incoming, outgoing | String | local |
| has_been_viewed | Näitab, kas dokumenti on vaadatud | Boolean | TRUE |
| document_type | Dokumendi tüüp (application / letter) | String | Application |
| creator_code | Dokumendi looja / omaniku kood | String | EE38407089745 |
| creator_name | Dokumendi looja / omaniku nimi | String | Eesnimi Perenimi |
| creator_user_code | Dokumendi loonud isiku kood (kasulik juhul, kui dokument on loodud asutuse kasutajakonto kaudu) | String | EE38407089745 |
| creator_user_name | Dokumendi loonud isiku nimi (kasulik juhul, kui dokument on loodud asutuse kasutajakonto kaudu) | String | Eesnimi Perenimi |
| created | Dokumendi loomise aeg | Date | 2010-08-09T12:17:26.000+03:00 |
| creator_application | Dokumendi loomisel kasutatud infosüsteemi nimi | String | KOV |
| last_mofified | Dokumendi viimase muutmise aeg | Date | 2010-08-09T12:17:26.000+03:00 |
| signable | Näitab, kas document on allkirjastatav | Boolean | TRUE |
| document_files | Dokumendi failide konteiner |  |  |
| remove_date | Dokumendi eeldatav kustutamise aeg. Arvutatakse viimase muutmise aja ja ADIT dokumentide säilitustähtaja alusel. | Date | 01/01/12 |
| signed | Näitab, kas dokumenti on allkirjastatud | Boolean | FALSE |
| eform_use_id | Evormi kasutuselevõtu id | Integer | 299 |
| file | Dokumendi faili andmed |  |  |
| file.id | Faili id (määratud juhul kui soovitakse muuta olemasolevat dokumendi faili) | Integer | 56 |
| file.name | Faili nimi koos laiendiga | String | Avaldus.txt |
| file.content_type | Faili MIME tüüp | String | Application/pdf |
| file.description | Faili pealkiri või kirjeldus | String | Avaldus Tallinna Linnavalitsusele |
| file.size_bytes | Faili suurus baitides | Long | 300 |
| file.file_type | Faili tüüp | String | document_file |
| file.data | Faili sisu Base64 kodeeringus | String | SvVnZXZhI… |

**Päringu vastuse SOAP manuse dekodeeritud sisu näide:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<document>
    <id>46</id>
    <guid>19d71714-0d93-292a-b254-3a06a4f24002</guid>
    <title>Avaldus Jõgeva Linnavalitsusele</title>
    <folder>local</folder>
    <has_been_viewed>true</has_been_viewed>
    <document_type>application</document_type>
    <creator_code>EE38407089745</creator_code>
    <creator_name>Eesnimi Perenimi</creator_name>
    <creator_user_code>EE38407089745</creator_user_code>
    <creator_user_name>Eesnimi Perenimi</creator_user_name>
    <created>2010-08-09T12:17:26.000+03:00</created>
    <creator_application>KOV</creator_application>
    <last_modified>2010-08-09T12:17:26.000+03:00</last_modified>
    <signable>true</signable>
    <remove_date>2012-01-01</remove_date>
    <document_files>
        <total_files>2</total_files>
        <total_bytes>446</total_bytes>
        <document_files>
            <file>
                <id>17</id>
                <name>avaldus2.txt</name>
                <content_type>text/plain</content_type>
                <description>Avaldus tekstifailina</description>
                <size_bytes>223</size_bytes>
                <file_type>document_file</file_type>
                <data>SvVnZXZhIExpbm5hdmFsaXRzdXMJMjAuMDQuMjAxMA0KDQpBdmFsZHVzDQoNClBhbHVuIHZpaXZp
dGFtYXR1bHQgdGFnYXN0YWRhIG1pbnUgcG9vbHQgbfb2ZHVudWQgdGVpc2lw5GV2YWwgbGlubmF2
YWxpdHN1c2Uga2FudHNlbGVpc3NlIHVudXN0YXR1ZCBrb2h2ZXIsIHN1dXNhZCBqYSByYW5uYXBh
bGwuDQoNCg0KRWlubyBNdWlkdWdpDQpBUyBBc3V0dXMNCkp1aGF0dXNlIGVzaW1lZXMNCg==
</data>
            </file>
            <file>
                <id>16</id>
                <name>avaldus1.txt</name>
                <content_type>text/plain</content_type>
                <description>Avaldus tekstifailina</description>
                <size_bytes>223</size_bytes>
                <file_type>document_file</file_type>
                <data>SvVnZXZhIExpbm5hdmFsaXRzdXMJMjAuMDQuMjAxMA0KDQpBdmFsZHVzDQoNClBhbHVuIHZpaXZp
dGFtYXR1bHQgdGFnYXN0YWRhIG1pbnUgcG9vbHQgbfb2ZHVudWQgdGVpc2lw5GV2YWwgbGlubmF2
YWxpdHN1c2Uga2FudHNlbGVpc3NlIHVudXN0YXR1ZCBrb2h2ZXIsIHN1dXNhZCBqYSByYW5uYXBh
bGwuDQoNCg0KRWlubyBNdWlkdWdpDQpBUyBBc3V0dXMNCkp1aGF0dXNlIGVzaW1lZXMNCg==
</data>
            </file>
        </document_files>
    </document_files>
</document>
```

## GetDocument.v2   

Päring võimaldab pärida dokumendi andmed. Dokumenti on võimalik pärida ka koos failidega (päringu vastuses tagastatakse ka dokumendi juurde kuuluvate failide sisu) – selleks tuleb määrata parameetri „include_file_contents“ väärtuseks „true“. 

Parameetriga „file_types“ saab piirata, millist tüüpi faile soovitakse alla laadida. Võimalikud väärtused:
   - document_file  =  dokumendi failid
   - signature_container  =  digitaalallkirja konteiner
   - zip_archive  =  dokumendi failidest moodustatud ZIP arhiiv
Tüüpi „zip_archive“ saab kasutada ainult juhul, kui „include_file_contents“ väärtuseks on „true“.

Võrreldes esimese versiooniga on lisatud võimalus filtreerida tagastatavaid dokumente ka parameetri DVK_ID järgi. Päringu vastuses tagastatakse võrreldes esimese versiooniga document_sharing.dvk_id veerg DVK ID-na (esimeses versioonis document.dvk_id veerg) .

Dokumendi andmed tagastatakse SOAP manusena. Viide SOAP manusele lisatakse vastuse parameetri „document“ atribuuti „href“.

Parameetrid

| Nimi | Väärtuse tüüp | Väärtuse näide |
| --- | --- | --- |
| Document_id | Integer | 46 |
| Dvk_id | Integer | 98 |
| Include_file_contents | Boolean | true |
| file_types | Faili tüüpide massiiv |  |
| file_types.file_type | Faili tüübi nimetus | signature_container |

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:amet="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd">
   <soapenv:Header>
      <amet:infosysteem>KOV</amet:infosysteem>
      <xtee:nimi>ametlikud-dokumendid.getDocument.v2</xtee:nimi>
      <xtee:id>00000000000000</xtee:id>
      <xtee:isikukood>EE38605150320</xtee:isikukood>
      <xtee:andmekogu>ametlikud-dokumendid</xtee:andmekogu>
      <xtee:asutus>10560025</xtee:asutus>
   </soapenv:Header>
   <soapenv:Body>
      <amet:getDocument>
         <keha>
            <dvk_id>9573</dvk_id>
             <include_file_contents>false</include_file_contents>
         </keha>
      </amet:getDocument>
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
            <id:serviceCode>getDocument</id:serviceCode>
            <id:serviceVersion>v2</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>

        <adit:infosysteem>KOV</adit:infosysteem>
    </soapenv:Header>
    <soapenv:Body>
        <amet:getDocument>
            <keha>
                <dvk_id>9573</dvk_id>
                <include_file_contents>false</include_file_contents>
            </keha>
        </amet:getDocument>
    </soapenv:Body>
</soapenv:Envelope>
```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<SOAP-ENV:Envelope SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/" xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/" xmlns:amet="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd">
   <SOAP-ENV:Header>
      <xtee:asutus xsi:type="xsd:string">10560025</xtee:asutus>
      <xtee:isikukood xsi:type="xsd:string">EE38605150320</xtee:isikukood>
      <xtee:id xsi:type="xsd:string">00000000000000</xtee:id>
      <xtee:nimi xsi:type="xsd:string">ametlikud-dokumendid.getDocument.v2</xtee:nimi>
      <xtee:andmekogu xsi:type="xsd:string">ametlikud-dokumendid</xtee:andmekogu>
   </SOAP-ENV:Header>
   <SOAP-ENV:Body>
      <amet:getDocumentResponse>
         <paring>
            <dvk_id>9573</dvk_id>
         </paring>
         <keha>
            <success>true</success>
            <messages>
               <message lang="et">Soovitud dokument edukalt väljastatud.</message>
               <message lang="en">Requested document was successfully retrieved.</message>
               <message lang="ru">Желаемый документ успешно выдан.</message>
            </messages>
            <document href="cid:761535221084975855247309434478"/>
         </keha>
      </amet:getDocumentResponse>
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
            <id:serviceCode>getDocument</id:serviceCode>
            <id:serviceVersion>v2</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <amet:getDocumentResponse>
            <keha>
                <success>true</success>
                <messages>
                    <message lang="et">Soovitud dokument edukalt väljastatud.</message>
                    <message lang="en">Requested document was successfully retrieved.</message>
                    <message lang="ru">Желаемый документ успешно выдан.</message>
                </messages>
                <document href="cid:761535221084975855247309434478"/>
            </keha>
        </amet:getDocumentResponse>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**Päringu vastuse SOAP manuse dekodeeritud sisu parameetrid:**

| Nimi | Kirjeldus | Tüüp | Väärtuse näide |
| --- | --- | --- | --- |
| Document | Dokumendi andmete konteiner |  |  |
| id | Dokumendi ID | Integer | 45 |
| guid | Dokumendi GUID | String | 19d71714-0d93-292a-b254-3a06a4f24002 |
| title | Dokumendi pealkiri | String |  |
| folder | Dokumendi kaust. Võimalikud väärtused: local, incoming, outgoing | String | local |
| has_been_viewed | Näitab, kas dokumenti on vaadatud | Boolean | TRUE |
| document_type | Dokumendi tüüp (application / letter) | String | Application |
| creator_code | Dokumendi looja / omaniku kood | String | EE38407089745 |
| creator_name | Dokumendi looja / omaniku nimi | String | Eesnimi Perenimi |
| creator_user_code | Dokumendi loonud isiku kood (kasulik juhul, kui dokument on loodud asutuse kasutajakonto kaudu) | String | EE38407089745 |
| creator_user_name | Dokumendi loonud isiku nimi (kasulik juhul, kui dokument on loodud asutuse kasutajakonto kaudu) | String | Eesnimi Perenimi |
| created | Dokumendi loomise aeg | Date | 2010-08-09T12:17:26.000+03:00 |
| creator_application | Dokumendi loomisel kasutatud infosüsteemi nimi | String | KOV |
| last_mofified | Dokumendi viimase muutmise aeg | Date | 2010-08-09T12:17:26.000+03:00 |
| dvk_id | Dokumendi DVK id (tagastatakse päringu tegija jaoks kehtiva DVK id) | Integer | 9573 |
| signable | Näitab, kas document on allkirjastatav | Boolean | TRUE |
| signed | Näitab, kas document on allkirjastatud | Boolean | TRUE |
| signatures | Dokumendi allkirjade konteiner |  |  |
| signature | Dokumendi allkirja andmed |  |  |
| signature .signer_code | Allkirjastaja kood | String | EE38407089745 |
| signature .signer_name | Allkirjastaja nimi | String | Juta Juust |
| signature .signing_date | Allkirjastamise kuupäev | Date | 014-01-10T15:59:31.000+02:00 |
| document_files | Dokumendi failide konteiner |  |  |
| remove_date | Dokumendi eeldatav kustutamise aeg. Arvutatakse viimase muutmise aja ja ADIT dokumentide säilitustähtaja alusel. | Date | 01/01/12 |
| file | Dokumendi faili andmed |  |  |
| file.id | Faili id (määratud juhul kui soovitakse muuta olemasolevat dokumendi faili) | Integer | 56 |
| file.name | Faili nimi koos laiendiga | String | Avaldus.txt |
| file.content_type | Faili MIME tüüp | String | Application/pdf |
| file.description | Faili pealkiri või kirjeldus | String | Avaldus Tallinna Linnavalitsusele |
| file.size_bytes | Faili suurus baitides | Long | 300 |
| file.file_type | Faili tüüp | String | document_file |
| file.data | Faili sisu Base64 kodeeringus | String | SvVnZXZhI… |

**Päringu vastuse SOAP manuse dekodeeritud sisu näide:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<document>
    <id>46</id>
    <guid>19d71714-0d93-292a-b254-3a06a4f24002</guid>
    <title>Avaldus Jõgeva Linnavalitsusele</title>
    <folder>local</folder>
    <has_been_viewed>true</has_been_viewed>
    <document_type>application</document_type>
    <creator_code>EE38407089745</creator_code>
    <creator_name>Eesnimi Perenimi</creator_name>
    <creator_user_code>EE38407089745</creator_user_code>
    <creator_user_name>Eesnimi Perenimi</creator_user_name>
    <created>2010-08-09T12:17:26.000+03:00</created>
    <creator_application>KOV</creator_application>
    <last_modified>2010-08-09T12:17:26.000+03:00</last_modified>
    <dvk_id>9573</dvk_id>
    <signable>true</signable>
    <remove_date>2012-01-01</remove_date>
    <signed>true</signed>
     <signatures>
          <signature>
              <signer_code>EE38407089745</signer_code>
              <signer_name>JUUST, JUTA</signer_name>
              <signing_date>2014-01-10T15:59:31.000+02:00</signing_date>
          </signature>
     </signatures>
    <document_files>
        <total_files>2</total_files>
        <total_bytes>446</total_bytes>
        <document_files>
            <file>
                <id>17</id>
                <name>avaldus2.txt</name>
                <content_type>text/plain</content_type>
                <description>Avaldus tekstifailina</description>
                <size_bytes>223</size_bytes>
                <file_type>document_file</file_type>
                <data>SvVnZXZhIExpbm5hdmFsaXRzdXMJMjAuMDQuMjAxMA0KDQpBdmFsZHVzDQoNClBhbHVuIHZpaXZp
dGFtYXR1bHQgdGFnYXN0YWRhIG1pbnUgcG9vbHQgbfb2ZHVudWQgdGVpc2lw5GV2YWwgbGlubmF2
YWxpdHN1c2Uga2FudHNlbGVpc3NlIHVudXN0YXR1ZCBrb2h2ZXIsIHN1dXNhZCBqYSByYW5uYXBh
bGwuDQoNCg0KRWlubyBNdWlkdWdpDQpBUyBBc3V0dXMNCkp1aGF0dXNlIGVzaW1lZXMNCg==
</data>
            </file>
            <file>
                <id>16</id>
                <name>avaldus1.txt</name>
                <content_type>text/plain</content_type>
                <description>Avaldus tekstifailina</description>
                <size_bytes>223</size_bytes>
                <file_type>document_file</file_type>
                <data>SvVnZXZhIExpbm5hdmFsaXRzdXMJMjAuMDQuMjAxMA0KDQpBdmFsZHVzDQoNClBhbHVuIHZpaXZp
dGFtYXR1bHQgdGFnYXN0YWRhIG1pbnUgcG9vbHQgbfb2ZHVudWQgdGVpc2lw5GV2YWwgbGlubmF2
YWxpdHN1c2Uga2FudHNlbGVpc3NlIHVudXN0YXR1ZCBrb2h2ZXIsIHN1dXNhZCBqYSByYW5uYXBh
bGwuDQoNCg0KRWlubyBNdWlkdWdpDQpBUyBBc3V0dXMNCkp1aGF0dXNlIGVzaW1lZXMNCg==
</data>
            </file>
        </document_files>
    </document_files>
</document>
```


## GetSendStatus.v1   

Päring annab võimalust teada saada kas DVK mõistes on dokument kohale jõudnud ja kellele dokument oli saadetud. Saatmise staaatust on võimalik pärida kasutades parameetri DHL id (ehk dokumendi DVK id järgi).

Dokumendi saajate andmed tagastatakse SOAP manusena. Viide SOAP manusele lisatakse vastuse parameetri „keha“ atribuuti „href“.

Parameetrid

| Nimi | Väärtuse tüüp | Väärtuse näide |
| --- | --- | --- |
| Document href | String | cid:123123123 |

### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:amet="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd">
   <soapenv:Header>
      <amet:infosysteem>KOV</amet:infosysteem>
      <xtee:nimi>ametlikud-dokumendid.getSendStatus.v1</xtee:nimi>
      <xtee:id>00000000000000</xtee:id>
      <xtee:isikukood>EE37507164211</xtee:isikukood>
      <xtee:andmekogu>ametlikud-dokumendid</xtee:andmekogu>
      <xtee:asutus>10560025</xtee:asutus>
   </soapenv:Header>
   <soapenv:Body>
      <amet:getSendStatus>
         <keha>
            <documendid href="cid:attachment.gz.b64"/>
         </keha>
      </amet:getSendStatus>
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
            <id:serviceCode>getSendStatus</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>

        <adit:infosysteem>KOV</adit:infosysteem>
    </soapenv:Header>
    <soapenv:Body>
        <amet:getSendStatus>
            <keha>
                <documendid href="cid:attachment.gz.b64"/>
            </keha>
        </amet:getSendStatus>
    </soapenv:Body>
</soapenv:Envelope>
```

**Päringu sisendi SOAP manuse dekodeeritud sisu parameetrid:**

| Nimi | Kirjeldus | Tüüp | Väärtuse näide |
| --- | --- | --- | --- |
| Item | Dokumentide dhl id-de konteiner |  |  |
| dhl_id | Dokumendi DVK ID | Integer | 45 |

**Näide*

```xml
<item>
	<dhl_id>2</dhl_id>
	<dhl_id>4</dhl_id>
</item>
```

### Päringu vastuse näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<SOAP-ENV:Envelope SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"
        xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"
        xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"
        xmlns:amet="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid" 
        xmlns:xsd="http://www.w3.org/2001/XMLSchema"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd">
    <SOAP-ENV:Header>
        <xtee:nimi>ametlikud-dokumendid.getSendStatus.v1</xtee:nimi>
        <xtee:id>00000000000000</xtee:id>
        <xtee:isikukood>EE37507164211</xtee:isikukood>
        <xtee:andmekogu>ametlikud-dokumendid</xtee:andmekogu>
        <xtee:asutus>10560025</xtee:asutus>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <amet:getSendStatusResponse>
            <paring>
                <documendid href="cid:attachment.gz.b64"/>
            </paring>
            <keha>
                <success>true</success>
                <messages>
                    <message lang="et">Soovitud staatused edukalt väljastatud.</message>
                    <message lang="en">Requested statuses were successfully retrieved.</message>
                    <message lang="ru">Желаемые статусы  успешно выданы.</message>
                </messages>
                <document href="cid:322724763013501706021551939037"/>
            </keha>
        </amet:getSendStatusResponse>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**X-tee sõnumiprotokoll v4.0**

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"
        xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid"
        xmlns:id="http://x-road.eu/xsd/identifiers"
        xmlns:xrd="http://x-road.eu/xsd/xroad.xsd">
    <SOAP-ENV:Header>
        <xrd:client id:objectType="MEMBER">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>BUSINESS</id:memberClass>
            <id:memberCode>12486864</id:memberCode>
        </xrd:client>
        <xrd:service id:objectType="SERVICE">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>
            <id:subsystemCode>adit</id:subsystemCode>
            <id:serviceCode>getSendStatus</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <adit:getSendStatusResponse>
            <keha>
                <success>true</success>
                <messages>
                    <message lang="et">Soovitud staatused edukalt väljastatud.</message>
                    <message lang="en">Requested statuses were successfully retrieved.</message>
                    <message lang="ru">Желаемые статусы  успешно выданы.</message>
                </messages>
                <document href="cid:790122391290409922891180468661"/>
            </keha>
        </adit:getSendStatusResponse>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**Päringu vastuse SOAP manuse dekodeeritud sisu parameetrid:**

| Nimi | Kirjeldus | Tüüp | Väärtuse näide |
| --- | --- | --- | --- |
| Item | Andmete konteiner |  |  |
| dokument | Dokumendi andmete konteiner |  |  |
| dhl_id | Dokumendi DHL ID | Integer | 45 |
| saaja | Dokumendi saaja andmete konteiner |  |  |
| isikukood | Saaja isikukood | String | 38407089745 |
| saajaNimi | Saaja nimi | String | Juta Juust |
| avatud | Kas saaja avas dokumendi | Boolean | TRUE |

**Näide:*

```xml
<?xml version="1.0" encoding="UTF-8"?>
<item>
    <dokument>
        <dhl_id>2</dhl_id>
        <saaja>
            <isikukood>EE75021729</isikukood>
            <saajaNimi>Kehtna Vallavalitsus</saajaNimi>
            <avatud>false</avatud>
        </saaja>
        <saaja>
            <isikukood>EE47101010033</isikukood>
            <saajaNimi>Mari-Liis Männik</saajaNimi>
            <avatud>false</avatud>
        </saaja>
    </dokument>
    <dokument>
        <dhl_id>4</dhl_id>
        <saaja>
            <isikukood>EE70000007</isikukood>
            <saajaNimi>Kodanikuportaal</saajaNimi>
            <avatud>false</avatud>
        </saaja>
    </dokument>
</item>
```

## GetDocumentFile.v1  

Päring võimaldab pärida dokumendi faili andmed – sisu ja metaandmed. Päringu parameetriga „document_id“ määratakse dokumendi ID ning parameetriga „file_id“ määratakse failid, mille andmeid soovitakse saada. Päringu vastuse SOAP manuses on dokumendi failide andmed.

Parameetrid

| Nimi | Väärtuse tüüp | Väärtuse näide |
| --- | --- | --- |
| Document_id | Integer | 46 |
| File_id_list.file_id | Integer | 16 |


### Päringu näide
---

**X-tee sõnumiprotokoll v2.0**

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd" xmlns:adit="http://producers.ametlikud-dokumendid.xtee.riik.ee/producer/ametlikud-dokumendid">
   <soapenv:Header>
      <xtee:nimi>ametlikud-dokumendid.getDocumentFile.v1</xtee:nimi>
      <xtee:id>00000000000000</xtee:id>
      <xtee:isikukood>EE38407089745</xtee:isikukood>
      <xtee:andmekogu>ametlikud-dokumendid</xtee:andmekogu>
      <xtee:asutus>12345678</xtee:asutus>
    <adit:infosysteem>KOV</adit:infosysteem>
   </soapenv:Header>
   <soapenv:Body>
      <adit:getDocumentFile>
         <keha>
            <document_id>46</document_id>
            <file_id_list>
               <file_id>16</file_id>
               <file_id>17</file_id>
            </file_id_list>
         </keha>
      </adit:getDocumentFile>
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
            <id:serviceCode>getDocumentFile</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>

        <adit:infosysteem>KOV</adit:infosysteem>
    </soapenv:Header>
    <soapenv:Body>
        <adit:getDocumentFile>
            <keha>
                <document_id>46</document_id>
                <file_id_list>
                    <file_id>16</file_id>
                    <file_id>17</file_id>
                </file_id_list>
            </keha>
        </adit:getDocumentFile>
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
      <xtee:isikukood xsi:type="xsd:string">EE38407089745</xtee:isikukood>
      <xtee:id xsi:type="xsd:string">00000000000000</xtee:id>
      <xtee:nimi xsi:type="xsd:string">ametlikud-dokumendid.getDocumentFile.v1</xtee:nimi>
      <xtee:andmekogu xsi:type="xsd:string">ametlikud-dokumendid</xtee:andmekogu>
   </SOAP-ENV:Header>
   <SOAP-ENV:Body>
      <adit:getDocumentFileResponse>
         <paring>
            <document_id>46</document_id>
            <file_id_list>
               <file_id>16</file_id>
               <file_id>17</file_id>
            </file_id_list>
         </paring>
         <keha>
            <success>true</success>
            <messages>
               <message lang="en">All requested files were successfully retrieved</message>
            </messages>
            <files href="cid:200611913490613646957590966591" />
        </keha>
      </adit:getDocumentFileResponse>
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
            <id:serviceCode>getDocumentFile</id:serviceCode>
            <id:serviceVersion>v1</id:serviceVersion>
        </xrd:service>
        <xrd:userId>EE37901130250</xrd:userId>
        <xrd:id>3cf04253-db0c-4d1d-8105-791472d88437</xrd:id>
        <xrd:protocolVersion>4.0</xrd:protocolVersion>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <adit:getDocumentFileResponse>
            <keha>
                <success>true</success>
                <messages>
                    <message lang="en">All requested files were successfully retrieved</message>
                </messages>
                <files href="cid:200611913490613646957590966591" />
            </keha>
        </adit:getDocumentFileResponse>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

**Päringu vastuse SOAP manuse dekodeeritud sisu parameetrid:**

| Nimi | Kirjeldus | Tüüp | Väärtuse näide |
| --- | --- | --- | --- |
| files | Failide konteiner |  |  |
| file | Faili andmete konteiner |  |  |
| file.id | Faili id (määratud juhul kui soovitakse muuta olemasolevat dokumendi faili) | Integer | 56 |
| file.name | Faili nimi koos laiendiga | String | Avaldus.txt |
| file.content_type | Faili MIME tüüp | String | Application/pdf |
| file.description | Faili pealkiri või kirjeldus | String | Avaldus Tallinna Linnavalitsusele |
| file.size_bytes | Faili suurus baitides | Long | 300 |
| file.file_type | Faili tüüp | String | document_file |
| file.data | Faili sisu Base64 kodeeringus | String | SvVnZXZhI… |

**Näide**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<getDocumentFileResponseAttachmentV1>
    <files>
        <file>
            <id>16</id>
            <name>avaldus1.txt</name>
            <content_type>text/plain</content_type>
            <description>Avaldus tekstifailina</description>
            <size_bytes>223</size_bytes>
            <file_type>document_file</file_type>
            <data>
SvVnZXZhIExpbm5hdmFsaXRzdXMJMjAuMDQuMjAxMA0KDQpBdmFsZHVzDQoNClBhbHVuIHZpaXZp
dGFtYXR1bHQgdGFnYXN0YWRhIG1pbnUgcG9vbHQgbfb2ZHVudWQgdGVpc2lw5GV2YWwgbGlubmF2
YWxpdHN1c2Uga2FudHNlbGVpc3NlIHVudXN0YXR1ZCBrb2h2ZXIsIHN1dXNhZCBqYSByYW5uYXBh
bGwuDQoNCg0KRWlubyBNdWlkdWdpDQpBUyBBc3V0dXMNCkp1aGF0dXNlIGVzaW1lZXMNCg==
</data>
        </file>
        <file>
            <id>17</id>
            <name>avaldus2.txt</name>
            <content_type>text/plain</content_type>
            <description>Avaldus tekstifailina</description>
            <size_bytes>223</size_bytes>
            <file_type>document_file</file_type>
            <data>
SvVnZXZhIExpbm5hdmFsaXRzdXMJMjAuMDQuMjAxMA0KDQpBdmFsZHVzDQoNClBhbHVuIHZpaXZp
dGFtYXR1bHQgdGFnYXN0YWRhIG1pbnUgcG9vbHQgbfb2ZHVudWQgdGVpc2lw5GV2YWwgbGlubmF2
YWxpdHN1c2Uga2FudHNlbGVpc3NlIHVudXN0YXR1ZCBrb2h2ZXIsIHN1dXNhZCBqYSByYW5uYXBh
bGwuDQoNCg0KRWlubyBNdWlkdWdpDQpBUyBBc3V0dXMNCkp1aGF0dXNlIGVzaW1lZXMNCg==
</data>
        </file>
    </files>
</getDocumentFileResponseAttachmentV1>
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
<a name="document-sign"></a>
## Dokumentide allkirjastamine

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
<a name="document-deliver"></a>
## Dokumentide saatmine

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
