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

|<xtee:nimi/> | päringu nimi. Nt. „ametlikud-dokumendid.join.v1“
|<xtee:id/> | päringu ID
|<xtee:isikukood/> | päringu teinud isiku kood
|<xtee:andmekogu/> | andmekogu nimi – „ametlikud-dokumendid“
|<xtee:asutus/> | päringut tegeva asutuse kood
|<adit:infosysteem/> | päringut tegeva infosüsteemi nimi

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

Dokumendi DVK staatused (DOCUMENT_DVK_STATUS). Päringud: _getDocumentList_.
|Väärtus | Selgitus/tähendus|
|-------|----------|
|100|Puudub|
|101|Ootel|
|102|Saatmisel|
|103|Saadetud|
|104|Katkestatud|
|105|Vastu võetud|

Dokumendi toimingud (DOCUMENT_HISTORY_TYPE). Päringud _getDocumentHistory_.
|Väärtus | Selgitus/tähendus|
|-------|----------|
|create|Dokumendi esmane loomine|
|modify|Dokumendi muutmine|
|add_file|Dokumendi faili lisamine|
|modify_file|Dokumendi faili muutmine|
|delete_file|Dokumendi faili kustutamine|
|modify_status|Dokumendi staatuse muutmine|
|send|Dokumendi saatmine|
|share|Dokumendi jagamine|
|lock|Dokumendi lukustamine|
|deflate|Dokumendi arhiveerimine (deflate)|
|sign|Dokumendi digitaalne allkirjastamine|
|delete|Dokumendi kustutamine|
|markViewed|Dokumendi vaadatuks märkimine|


## Õiguste süsteem

## Join.v1
 
## UnJoin.v1

## GetJoined.v1

## SetNotifications.v1  

## GetNotifications.v1   

## SaveDocument.v1  

## SaveDocumentFile.v1  

## DeflateDocument.v1   
## DeleteDocument.v1   
## DeleteDocumentFile.v1   
## GetDocument.v1   
## GetDocument.v2   
## GetSendStatus.v1   
## GetDocumentFile.v1   
## GetDocumentHistory.v1   
## GetDocumentList.v1   
## PrepareSignature.v1   
## ConfirmSignature.v1   
## SendDocument.v1   
## ShareDocument.v1   
## UnShareDocument.v1   
## MarkDocumentViewed.v1   
## ModifyStatus.v1   
## GetUserContacts.v1 