#Dokumendivahetuskeskuse (DVK) Liideste spetsifikatsioon

##Sisukord
- [Muudatuste ajalugu](#muudatuste-ajalugu)
- [Sissejuhatus](#sissejuhatus)
- [Üldskeem: dokumendid, metainfo, dokumendivahetus](#yldskeem-dokumendid-metainfo-dokumendivahetus)
- [Vahetatava dokumendi üldine XML kirjeldus](#vahetatava-dokumendi-yldine-xml-kirjeldus)
    - [Dokumendi üldine ümbrik]()
    - [Metainfo blokk dokumendis]()
    - [Transport blokk dokumendis]()
    - [DVK konteineri versioon 2 puhul on kasutusel järgmine struktuur]()
    - [Ajalugu blokk dokumendis]()
    - [Metaxml blokk dokumendis]()
    - [Taustinfoks: DigiDoci konteiner]()
    - [Taustinfoks: `<failid>` konteiner]()
- [Dokumentide logistika]()
    - [Üldist]()
    - [Tööskeem]()
        - [DVK loogiline ülesehitus]()
        - [Kaustade kasutamine]()
        - [Dokumentide edastamine]()
        - [Edastatud dokumentide staatuse kontroll]()
        - [Dokumentide vastuvõtt]()
- [X-Tee päringute kirjeldused]()
    - [sendDocuments]()
        - [sendDocuments.v1]()
        - [sendDocuments.v2]()
        - [sendDocuments.v3]()
        - [sendDocuments.v4]()
    - [getSendStatus]()
        - [getSendStatus.v1]()
        - [getSendStatus.v2]()
    - [receiveDocuments]()
        - [receiveDocuments.v1]()
        - [receiveDocuments.v2]()
        - [receiveDocuments.v3]()
        - [receiveDocuments.v4]()
    - [markDocumentsReceived]()
        - [markDocumentsReceived.v1]()
        - [ markDocumentsReceived.v2]()
        - [markDocumentsReceived.v3]()
    - [getSendingOptions]()
        - [getSendingOptions.v1]()
        - [getSendingOptions.v2]()
        - [getSendingOptions.v3]()
    - [changeOrganizationData]()
        - [Näide]()
    - [deleteOldDocuments]()
        - [Näide]()
    - [runSystemCheck]()
        - [Näide]()
    - [getSubdivisionList]()
        - [getSubdivisionList.v1]()
        - [getSubdivisionList.v2]()
    - [getOccupationList]()
        - [getOccupationList.v1]()
        - [getOccupationList.v2]()
- [Kasutusõiguste süsteem DVK rakenduses]()
- [Edastatavate dokumentide valideerimine DVK serveris]()
- [Adressaatide automaatne lisamine DVK serveris]()
    - [Adressaatide automaatse lisamise seadistamine]()
- [Dokumentide edastamine DVK serverite vahel (DVK lüüsid)]()
    - [Sissejuhatus]()
    - [Tehnilised piirangud DVK lüüside kasutamisele]()
    - [DVK lüüside lahendusest tingitud muudatused DVK spetsifikatsioonis]()
        - [Vahendaja kirje DVK konteineri transport andmestruktuuris]()
        - [DVK serveri täiendavad seadistused]()
- [Dokumentide edastamine fragmentidena]()
- [Teadaolevad vead DVK rakenduses]()
    - [Content-Transfer-Encoding päise vigane esitus]()
    - [Tundlikkus Content-Type päise kirjapildi suhtes]()
- [LISA 1: Kasutatavate andmete XML Schema kirjeldused]()
    - [Automaatsed metaandmed]()
    - [Manuaalsed metaandmed]()
    - [DVK dokument]()
    - [Päringute WSDL kirjeldus]()
- [LISA 2: <dokument> XML struktuuri kasutusnäide (DVK konteineri versioon 1)]()
- [LISA 3: <dokument> XML struktuuri kasutusnäide (DVK konteineri versioon 2)]()



##Muudatuste ajalugu

| Kuupäev | Versioon | Kirjeldus | Autor |
|-------|----------|----------------|----------------------|
| 26.11.2005  | 1.1 | Dokumendi esialgne versioon | OÜ Degeetia |
| 17.04.2006 | 1.3.1 | Terminid „dokumendirepositoorium”, „dokumendihoidla” ning vastavad lühendid „DR” ja „DHL” on asendatud (v.a tehniline osa päringute kirjeldustes) „dokumendivahetuskeskus” ja „DVK”-ga | Rauno Temmer (RIA) |


##Sissejuhatus
Dokumendivahetuskeskus (DVK) on erinevatele dokumendihaldussüsteemidele ja muudele dokumente käsitlevatele infosüsteemidele ühine keskset dokumendivahetuse teenust pakkuv infosüsteem. DVK ülesanne on hajutatult paiknevate infosüsteemide liidestamine X-tee vahendusel, dokumentide lühiajaline säilitamine ja lähimas tulevikus ka dokumentide menetlemist toetavate teenuste pakkumine.

Spetsifikatsioonile on eraldi lisatud kolm lisa:
- [Lisa 1. Viited kasutatavatele nimeruumidele ja XML skeemikirjeldustele]()
- [Lisa 2. DVK dokumendi näide (DVK konteineri versioon 1)]()
- [Lisa 3. DVK dokumendi näide (DVK konteineri versioon 2)]()

##Üldskeem: dokumendid, metainfo, dokumendivahetus

Dokumendivahetus DHS ja DVK vahel toimub järgmiste põhikonteinerite kaudu:
- Väline XTEE SOAP-ümbrik sisaldab gzip-ga kokkupakitud, seejärel base64 kodeeritud XML  konteinerit "dokument"
    - "dokument" konteiner sisaldab (peale base64-dekodeerimist ja gzip-lahtipakkimist):
        - eri tüüpi metainfo- ja transpordi-info konteinereid
        - DigiDoc formaadis dokumenti (DVK konteiner versioon 1), mis omakorda:
            - sisaldab suvalise hulga base64 kodeeringus ja minimaalse formaadi-metainfoga varustatud faile
            - sisaldab suvalise hulga allkirja-blokke nimetatud failide jaoks. Allkirja-blokkidesse ei tule kopeerida konteineris olevate eraldi allkirjastatud dokumentide allkirja-blokke. NB! DVK kontekstis on lubatud allkirja-blokkide puudumine!
        - <failid> formaadis konteinerit (DVK konteiner versioon 2), mis omakorda:
            - Sisaldab suvalise hulga faile zip-itud ja seejärel Base64 kodeeringusse panduna. Lisaks failide metaandmed.

Dokumendi ("dokument" konteiner) struktuuri kirjeldame järgmises peatükis, SOAP-konteinerit ja X-Tee päringut aga edasises "Dokumentide logistika" peatükis.

##Vahetatava dokumendi üldine XML kirjeldus

Dokumendi formaat põhineb kas DigiDoc formaadil (DVK konteineri versioon 1) või `<failid>` konteineril. Dokumendi kohta on olemas erinevat tüüpi metainfo pluss suvaline hulk DigiDoc/failid konteinereid. Muud kaalutlused:

- Osa dokumendi XML formaadi välju on kriitilised dokumentide logistika ja/või kasutajaliidese põhifunktsionaalsuse jaoks, osa aga ei ole DVK tööks otseselt vajalikud. Pakutav formaat fikseerib konkreetsete tagidena (st sisaldab nimeruumis) ainult kriitilised tagid, kõiki muid metaandmeid võib formaati samuti lisada - suvaliste nimedega, nn kasutaja-poolt defineeritud väljadena, mis ei ole otseselt meie formaadi nimeruumis.

- XML formaadi kasutamise korral ei ole väga oluline, mis on konkreetselt tagide ja atribuutide nimed: oluline on, et tagide oodatav sisu oleks piisavalt lahti selgitatud, ning oleks suhteliselt kerge mõista, mida mingisse tagi panna. Seetõttu oleme läbivalt kasutanud võimalikult selgelt mõistetavaid eestikeelseid nimesid tagidele, mitte aga näiteks Dublin Core ingliskeelseid nimesid.

- Kõik metainfo väljad moodustavad sisuliselt "lameda", struktuurita loetelu rdf-ideoloogiaga sobivatest nimi-väärtus paaridest, mida on lihtne realiseerida ja laiendada.

- Metainfo väljad ei ole allkirjastatud. Allkirjad on ainult dokumentide küljes DigiDoc konteineris. Seejuures on allkirjastatud dokumendil DigiDoc formaadis siiski olemas väike hulk spetsiaalseid metainfo-välju (dokumendi formaat jne), mis on alati allkirjastatud. NB! DigiDoc formaati kasutatakse ainult DVK konteineri versiooni 1 puhul. DVK konteineri versioon 2 kasutab <failid> konteinerit, millel ei ole allkirja hoidmiseks spetsiaalset struktuuri. Allkirjastatud dokumentide edastamiseks saab sellisel juhul lisada allkirjastatud dokumendi <failid> konteinerisse.

- Kuupäevi ja kellaaegu esitatakse XML struktuuris ISO8601 standardile vastavalt (http://www.w3.org/TR/NOTE-datetime). S.t. kuupäev+kellaaeg kujul YYYY-MM-DDThh:mm:ssTZD (näit: 2006-03-20T17:25:00+02:00) ning kuupäev kujul YYY-MM-DD (näit: 2006-03-20)

Juhime veel tähelepanu, et DVK konteineri versiooni 1 (kasutab failide kirjeldamiseks DigiDoc konteinerit):

- DigiDoc ei pea sisaldama allkirja.
- Üks DigiDoc võib sisaldada mitmeid konkreetseid dokumente.
- DigiDoc sees olevate konkreetsete dokumentide allkirja-blokke hoitakse dokumendi juures, mitte „dokument“ konteineri DigiDoc-is
- Iga sisalduv konkreetne dokument võib sisaldada metadata atribuute, mis signeeritakse.
- Lisatavaid metadata blokke ei signeerita.


Alates DVK versioonist 1.6.0 on toetatud ka DVK konteineri versioon 2, milles faile hoitakse `<failid>` konteineris. `<failid>` konteiner sisaldab omakorda suvalise arvu faile ja nende metaandmeid. DVK konteineri versioon 2 ei toeta otse failide konteineri küljes olevat digiallkirja. Digiallkirjastatud dokumentide edastamiseks tuleb `<failid>` konteinerisse lisada allkirjastatud DigiDoc fail.


###Dokumendi üldine ümbrik

DVK-le saadetavad ja sealt loetavad dokumendid on järgmisel kujul XML-tekstid (detailsemalt kirjeldatakse infoblokke järgmistes peatükkides):

```xml
<?xml version="1.0" encoding="UTF-8" ?><dhl:dokument xmlns:dhl="http://www.riik.ee/schemas/dhl">
 
 <!-- Konkreetse struktuuri/semantikaga metainfo blokk,
      Sisaldab nii DVK tarkvara kui kasutaja poolt sisestatut.
      Siia blokki pannakse metainfo, millest teised süsteemid
      peavad aru saama.
      Võib saatja poolt jääda tühjaks/puududa.            
 -->
 <dhl:metainfo/>
 
 <!-- Konkreetse semantikaga transpordiblokk, sisaldab konkreetset kust/kuhu
      infot, mille kaudu DVK tarkvara dokumendi edasi saadab.
      Edasisaatmiseks mõeldud dokumentide juures kohustuslik osa.
 -->
 <dhl:transport/>
 
 <!-- Informatsioon dokumendi senise liikumistee ja muutumiste kohta.
      Blokk sisaldab lihtsalt varasemaid metainfo, transport ja metaxml
      blokke, järjestikuselt. Dokumendi edasisaatmisel kopeeritakse viimased
      nimetatud blokid ajaloo bloki lõppu. Tarkvara võib kopeerimise käigus
      otsustada mõned bloki osad (või terved blokid) kopeerimata jätta.
      Esialgse saatja poolt jäetakse üldjuhul tühjaks.
 -->
 <dhl:ajalugu/>
 
 <!—- Järgnevad dokumendi liigist sõltuva struktuuriga metaandmed.
      Soovituslik oleks igal konkreetsel juhul viidata metaandmete struktuuri
      kirjeldavale XML skeemile (schema), kasutada selleks xmlns ja schemaLocation
      atribuute. Vaikimisi eeldatakse, et metaxml blokis sisalduvad andmed vastavad
      Riigikantselei poolt fikseeritud kirja metaandmete vormingule:
      (http://www.riik.ee/schemas/dhl/rkel_letter.xsd)
 -->
 <dhl:metaxml xmlns="http://www.riik.ee/schemas/dhl/rkel_letter"
   schemaLocation="http://www.riik.ee/schemas/dhl/rkel_letter
   http://www.riik.ee/schemas/dhl/rkel_letter.xsd"/>
 
 <!-- Järgneb üks SignedDoc struktuur, kas siis signatuuridega või ei.
      SignedDoc blokk sisaldab konkreetseid dokumente.
      Kirjeldus vastab täpselt AS Sertifitseerimiskeskuse antud
      kirjeldusele, täiendavat infot järgnevas.
 -->

 <!-- DVK konteiner versioon 1 -->
 <SignedDoc/>

 <!-- DVK konteiner versioon 2 -->
 <dhl:failid/>

</dhl:dokument>
```

Järgnev on DigiDoc formaadi ümbriku ülemise taseme kirjeldus AS Sertifitseerimiskeskuselt (SK). DataFile ja Signature blokkide sisu struktuuri saab lugeda SK kodulehe kaudu leitavalt DigiDoc lehelt.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<SignedDoc format="DIGIDOC-XML" version="1.3"           
           xmlns="http://www.sk.ee/DigiDoc/v1.3.0#">       
    <!-- Optsionaalsed algandmefailid: DigiDoc formaadi järgi -->
    <DataFile/>
    .....
    <DataFile/>
    <!-- Optsionaalsed: kliendi allkirjad koos kehtivuskinnitustega,
         DigiDoc formaadi järgi -->
    <Signature/>
    ....
    <Signature/>
</SignedDoc>

Kui kasutatakse DVK konteineri versiooni 2, siis hoitakse faile <failid> konteineris, mille struktuur on järgmine:

<dhl:failid xmlns="http://www.riik.ee/schemas/dhl/2010/2">       
<fail>
        <jrknr/>
      <fail_pealkiri/>
      <fail_suurus/>
      <fail_tyyp/>
      <fail_nimi/>
      <zip_base64_sisu/>
      <krypteering/>
      <pohi_dokument/>
      <pohi_dokument_konteineris/>
    </fail>
</dhl:failid>
```

Kui kasutatakse DVK konteineri versiooni 2, siis hoitakse faile `<failid>` konteineris, mille struktuur on järgmine:

```
<dhl:failid xmlns="http://www.riik.ee/schemas/dhl/2010/2">       
<fail>
        <jrknr/>
      <fail_pealkiri/>
      <fail_suurus/>
      <fail_tyyp/>
      <fail_nimi/>
      <zip_base64_sisu/>
      <krypteering/>
      <pohi_dokument/>
      <pohi_dokument_konteineris/>
    </fail>
</dhl:failid>
```

###Metainfo blokk dokumendis

MetaInfo bloki struktuur on lame: ta sisaldab hulgaliselt rdf-ideoloogia järgi väli-väärtus paare, kus iga paar annab kogu dokumendi ümbriku jaoks mingi väärtuse.

Metainfo väljade nimede juures otsustasime lähtuda esmajoones Eesti dokumendihaldussüsteemide ja ametiasutuste tüüpvajadustes ja asjaajamiskorrast ning -printsiipidest, mitte aga otseselt Dublin Core väljadest. Rõhutame, et vajadusel on XML-süntaksis välju väga lihtne automaatselt tõlkida teistes keeltes väljadeks.

-   Ükski metainfo bloki väli (erinevalt transpordi bloki väljadest) ei ole kohustuslik.
-   Osa metainfo välju tuleb kasutajalt/DHS-st, osa aga täidab DVK automaatselt.
-   DVK peaks pakkuma võimalust teostada dokumentide otsingut ja võib-olla ka muid teenuseid, baseerudes nimetatud väljade sisule (DVK realisatsioonilt võiks oodata, et nimetatud väljade sisu paigutatakse igaüks eraldi andmebaasiväljale).
-   Väljade üks eesmärk on olla nö vahekeeleks, mille kaudu erinevad DHS-s oma metainfot ekspordivad ja impordivad.

Metainfo väljad jaotuvad järgmiselt:

-   DVK tarkvara poolt automaatselt täidetavad väljad. Need väljad on nimeruumis [http://www.riik.ee/schemas/dhl-meta-automatic](http://www.riik.ee/schemas/dhl-meta-automatic)
-   DVK tarkvara poolt automaatselt täidetavad väljad, mis on võetud e-kirja päistest: ainult juhul, kui dokument on DVK-le saadetud e-postiga. Need väljad on nimeruumis http://www.riik.ee/schemas/dhl-meta-automatic.
-   Väljad, mille olemasolu korral omavad välja sisu konkreetset tähendust repositooriumi jaoks. Need väljad on järgmistes nimeruumides sõltuvalt kasutatavast DVK konteineri versioonist:
    - DVK konteiner versioon 1 - [http://www.riik.ee/schemas/dhl-meta-manual](http://www.riik.ee/schemas/dhl-meta-manual)
    - DVK konteiner versioon 2 - [http://www.riik.ee/schemas/dhl-meta-manual/2010/2](http://www.riik.ee/schemas/dhl-meta-manual/2010/2)
-   Mistahes muud väljad, mida dokumendi DVK-sse lisav tarkvara soovib metainfosse lisada. Nende väljade kodeeringu viis on erinev, kui eelmistel, DVK jaoks tähendust omavatel väljadel. Ainus siin kasutatav tag on nimeruumis [http://www.riik.ee/schemas/dhl-meta-manual](http://www.riik.ee/schemas/dhl-meta-manual) (DVK konteineri versioon 1)

```xml
<dhl:metainfo xmlns:dhl="http://www.riik.ee/schemas/dhl"
              xmlns:ma="http://www.riik.ee/schemas/dhl-meta-automatic"  
              xmlns:mm="http://www.riik.ee/schemas/dhl-meta-manual">  

<!-- DVK tarkvara poolt alati,
     automaatselt täidetavad väljad, kusjuures
     teine blokk on kasutusel xteega saadud/saadetud
     dokumentide, kolmas aga epostiga saadud/saadetud
     dokumentide jaoks -->
     
 <ma:dhl_id>DVK sisene unikaalne id</ma:dhl_id>
 <ma:dhl_kaust>Kausta, kus antud dokumenti DVK-s hoitakse, nimi</ma:dhl_kaust>
 <ma:dhl_saabumisviis>esialgu string email või string xtee</ma:dhl_saabumisviis>
 <ma:dhl_saabumisaeg>DVK -sse saabumise aeg CCYY-MM-DDThh:mm:ss</ma:dhl_saabumisaeg>  
 <ma:dhl_saatmisviis>esialgu string email või string xtee</ma:dhl_saatmisviis>
 <ma:dhl_saatmisaeg>DVKst saatmise aeg CCYY-MM-DDThh:mm:ss</ma:dhl_saatmisaeg>
 
 <ma:dhl_saatja_asutuse_nr>
   registrinr turvaserverist (ainult xteest tulnud)
 </ma:dhl_saatja_asutuse_nr>
 <ma:dhl_saatja_asutuse_nimi>
   asutuse nimi turvaserverist (ainult xteest tulnud)
 </ma:dhl_saatja_asutuse_nr>
 <ma:dhl_saatja_isikukood>
  isikukood turvaserverist (ainult xteest tulnud)
 </ma:dhl_saatja_isikukood>
 <ma:dhl_saaja_asutuse_nr>
   registrinr turvaserverile (xteega saadetud)
 </ma:dhl_saaja_asutuse_nr>
 <ma:dhl_saaja_asutuse_nimi>
   asutuse nimi turvaserverile (xteega saadetud)
 </ma:dhl_saaja_asutuse_nr>
 <ma:dhl_saaja_isikukood>
  saaja kood turvaserverile (xteega saadetud)
 </ma:dhl_saaja_isikukood>
 
 <ma:dhl_saatja_epost>
  saatja eposti aadress (tulnud epostiga)
 </ma:dhl_saatja_epost>
 <ma:dhl_saaja_epost>
  epost, kellele saadeti (saadetud epostiga)
 </ma:dhl_saaja_epost>


<!-- DVK tarkvara poolt automaatselt täidetavad väljad,
     mis on võetud emaili headeritest: ainult juhul, kui
     dokument on DVK -sse saadetud mailiga -->

 <ma:dhl_email_header  ma:name="E-posti headeri välja nimi">
   Sisu
 </ma:dhl_email_header>


<!-- väljad, mille olemasolu korral metainfo blokis
     omab välja sisu konkreetset tähendust DVK jaoks
     ning seda saab kasutada mh otsingutes ja kasutajaliideses.

    Siin on toodud eraldi info koostaja kohta ja info
    saatja kohta: viimast tuleb kasutada, kui saatja (asutus)
    on erinev koostajast (asutusest)
  
 -->
 
 <mm:koostaja_asutuse_nr>
    algse koostaja (autori) asutuse number
 </mm:koostaja_asutuse_nr>
 <mm:saaja_asutuse_nr>
   asutuse nr, kellele saata (kui tühi, ei saadeta)
 </mm:saaja_asutuse_nr>
 
 <mm:koostaja_dokumendinimi>
   dokumendi nimi koostajal
 </mm:koostaja_doknimi>
 <mm:koostaja_dokumendityyp>
  dokumendi tüüp koostajal (leping vms)
 </mm:koostaja_doktyyp>
 <mm:koostaja_dokumendinr>
   koostaja asutuse dokumendinumber
 </mm:koostaja_dokumendinr>
 <mm:koostaja_failinimi>
   failinimi koostajal (ilma kataloogide-teeta)
 </mm:koostaja_kataloog>
 <mm:koostaja_kataloog>
   kataloog, kus koostaja dokumenti hoidis (täistee, ilma dokumendifaili nimeta)
 </mm:koostaja_kataloog>
 <mm:koostaja_votmesona>
  dokumendi võtmesõna koostajal
 </mm:koostaja_votmesona>
 <mm:koostaja_kokkuvote>
   dokumendi sisu/eesmärgi lühike kokkuvõte (abstract)
 </mm:koostaja_kokkuvote>
 <mm:koostaja_kuupaev>
   koostamiskuupäev
 </mm:koostaja_kuupaev>
 <mm:koostaja_asutuse_nimi>
  algse koostaja asutuse nimi
 </mm:koostaja_asutuse_nimi>
 <mm:koostaja_asutuse_kontakt>
  algse koostaja asutuse kontaktinfo
 </mm:koostaja_asutuse_kontakt>
  
 <mm:autori_osakond>
   osakond, kus autor ehk ametlik saatja peaks töötama
 </mm:autori_osakond>
 <mm:autori_isikukood>
  autori ehk ametliku saatja isikukood
 </mm:autori_isikukood>
 <mm:autori_nimi>
  autori ehk ametliku saatja nimi
 </mm:autori_nimi>
 <mm:autori_kontakt>
  autori ehk ametliku saatja kontaktinfo
 </mm:autori_kontakt>
 
 <mm:seotud_dokumendinr_koostajal>
  Koostaja dokument, mis on praegusega seotud
 </mm:seotud_dokumendinr_koostajal>
 <mm:seotud_dokumendinr_saajal>
  Saaja dokument, mis on praegusega seotud
  </mm:seotud_dokumendinr_saajal>

 <mm:saatja_dokumendinr>
  saatja asutuse dokumendinumber (kui erineb koostajast)
 </mm:saatja_dokumendinr>
 <mm:saatja_kuupaev>
  saatmiskuupäev (kui erineb koostamiskuupäevast)
 </mm:saatja_kuupaev>
 <mm:saatja_asutuse_kontakt>
  saatja asutuse kontakt
 </mm:saatja_asutuse_kontakt>
  
 <mm:saaja_isikukood>
  isikukood, kellele dokument saadetakse
 </mm:saaja_isikukood>
 <mm:saaja_nimi>
  isiku nimi, kellele dokument saadetakse
 </mm:saaja_nimi>
 <mm:saaja_osakond>
  osakonna nimi, kus vastav isik peaks töötama
 </mm:saaja_osakond>
 <mm:seotud_dhl_id>
  Seotud dokumendi DVK ID.
 </mm:seotud_dhl_id>
 <mm:sisu_id>
  Põhifaili ID, kui SignedDoc konteineris saadetakse mitu faili.
  Vajalik selleks, et saaks eristada põhifaili ja lisafaile.
 </mm:sisu_id>
<!--
  mistahes muud väljad, mida dokumendi DVK-sse lisav
  tarkvara soovib metainfosse lisada. Nende väljade kodeeringu
  viis on erinev, kui eelmistel, DVK jaoks tähendust
  omavatel väljadel.
-->

  <mm:saatja_defineeritud mm:valjanimi="Mingi nimi">
   Mingi väärtus
  </mm:saatja_defineeritud>

</dhl:metainfo>
```

Kui kasutusel on DVK konteineri versioon 2, siis on kasutatavaks nimeruumiks [http://www.riik.ee/schemas/dhl-meta-manual/2010/2](http://www.riik.ee/schemas/dhl-meta-manual/2010/2).
DVK konteineri versioon 2 metainfo blokk näeb välja järgmine:

```xml
<dhl:metainfo xmlns:dhl="http://www.riik.ee/schemas/dhl/2010/2"
              xmlns:ma="http://www.riik.ee/schemas/dhl-meta-automatic"  
              xmlns:mm="http://www.riik.ee/schemas/dhl-meta-manual/2010/2">  

<!-- DVK tarkvara poolt alati,
     automaatselt täidetavad väljad, kusjuures
     teine blokk on kasutusel xteega saadud/saadetud
     dokumentide, kolmas aga epostiga saadud/saadetud
     dokumentide jaoks -->
     
 <ma:dhl_id>DVK sisene unikaalne id</ma:dhl_id>
 <ma:dhl_kaust>Kausta, kus antud dokumenti DVK-s hoitakse, nimi</ma:dhl_kaust>
 <ma:dhl_saabumisviis>esialgu string email või string xtee</ma:dhl_saabumisviis>
 <ma:dhl_saabumisaeg>DVK -sse saabumise aeg CCYY-MM-DDThh:mm:ss</ma:dhl_saabumisaeg>  
 <ma:dhl_saatmisviis>esialgu string email või string xtee</ma:dhl_saatmisviis>
 <ma:dhl_saatmisaeg>DVKst saatmise aeg CCYY-MM-DDThh:mm:ss</ma:dhl_saatmisaeg>
 
 <ma:dhl_saatja_asutuse_nr>
   registrinr turvaserverist (ainult xteest tulnud)
 </ma:dhl_saatja_asutuse_nr>
 <ma:dhl_saatja_asutuse_nimi>
   asutuse nimi turvaserverist (ainult xteest tulnud)
 </ma:dhl_saatja_asutuse_nr>
 <ma:dhl_saatja_isikukood>
  isikukood turvaserverist (ainult xteest tulnud)
 </ma:dhl_saatja_isikukood>
 <ma:dhl_saaja_asutuse_nr>
   registrinr turvaserverile (xteega saadetud)
 </ma:dhl_saaja_asutuse_nr>
 <ma:dhl_saaja_asutuse_nimi>
   asutuse nimi turvaserverile (xteega saadetud)
 </ma:dhl_saaja_asutuse_nr>
 <ma:dhl_saaja_isikukood>
  saaja kood turvaserverile (xteega saadetud)
 </ma:dhl_saaja_isikukood>
 
 <ma:dhl_saatja_epost>
  saatja eposti aadress (tulnud epostiga)
 </ma:dhl_saatja_epost>
 <ma:dhl_saaja_epost>
  epost, kellele saadeti (saadetud epostiga)
 </ma:dhl_saaja_epost>


<!-- DVK tarkvara poolt automaatselt täidetavad väljad,
     mis on võetud emaili headeritest: ainult juhul, kui
     dokument on DVK -sse saadetud mailiga -->

 <ma:dhl_email_header  ma:name="E-posti headeri välja nimi">
   Sisu
 </ma:dhl_email_header>


<!-- väljad, mille olemasolu korral metainfo blokis
     omab välja sisu konkreetset tähendust DVK jaoks
     ning seda saab kasutada mh otsingutes ja kasutajaliideses.

    Siin on toodud eraldi info koostaja kohta ja info
    saatja kohta: viimast tuleb kasutada, kui saatja (asutus)
    on erinev koostajast (asutusest)
  
 -->
 
 <mm:koostaja_asutuse_nr>
    algse koostaja (autori) asutuse number
 </mm:koostaja_asutuse_nr>
 <mm:saaja_asutuse_nr>
   asutuse nr, kellele saata (kui tühi, ei saadeta)
 </mm:saaja_asutuse_nr>
 
 <mm:koostaja_dokumendinimi>
   dokumendi nimi koostajal
 </mm:koostaja_doknimi>
 <mm:koostaja_dokumendityyp>
  dokumendi tüüp koostajal (leping vms)
 </mm:koostaja_doktyyp>
 <mm:koostaja_dokumendinr>
   koostaja asutuse dokumendinumber
 </mm:koostaja_dokumendinr>
 <mm:koostaja_failinimi>
   failinimi koostajal (ilma kataloogide-teeta)
 </mm:koostaja_kataloog>
 <mm:koostaja_kataloog>
   kataloog, kus koostaja dokumenti hoidis (täistee, ilma dokumendifaili nimeta)
 </mm:koostaja_kataloog>
 <mm:koostaja_votmesona>
  dokumendi võtmesõna koostajal
 </mm:koostaja_votmesona>
 <mm:koostaja_kokkuvote>
   dokumendi sisu/eesmärgi lühike kokkuvõte (abstract)
 </mm:koostaja_kokkuvote>
 <mm:koostaja_kuupaev>
   koostamiskuupäev
 </mm:koostaja_kuupaev>
 <mm:koostaja_asutuse_nimi>
  algse koostaja asutuse nimi
 </mm:koostaja_asutuse_nimi>
 <mm:koostaja_asutuse_kontakt>
  algse koostaja asutuse kontaktinfo
 </mm:koostaja_asutuse_kontakt>
  
 <mm:autori_osakond>
   osakond, kus autor ehk ametlik saatja peaks töötama
 </mm:autori_osakond>
 <mm:autori_isikukood>
  autori ehk ametliku saatja isikukood
 </mm:autori_isikukood>
 <mm:autori_nimi>
  autori ehk ametliku saatja nimi
 </mm:autori_nimi>
 <mm:autori_kontakt>
  autori ehk ametliku saatja kontaktinfo
 </mm:autori_kontakt>
 
 <mm:seotud_dokumendinr_koostajal>
  Koostaja dokument, mis on praegusega seotud
 </mm:seotud_dokumendinr_koostajal>
 <mm:seotud_dokumendinr_saajal>
  Saaja dokument, mis on praegusega seotud
  </mm:seotud_dokumendinr_saajal>

 <mm:saatja_dokumendinr>
  saatja asutuse dokumendinumber (kui erineb koostajast)
 </mm:saatja_dokumendinr>
 <mm:saatja_kuupaev>
  saatmiskuupäev (kui erineb koostamiskuupäevast)
 </mm:saatja_kuupaev>
 <mm:saatja_asutuse_kontakt>
  saatja asutuse kontakt
 </mm:saatja_asutuse_kontakt>
  
 <mm:saaja_isikukood>
  isikukood, kellele dokument saadetakse
 </mm:saaja_isikukood>
 <mm:saaja_nimi>
  isiku nimi, kellele dokument saadetakse
 </mm:saaja_nimi>
 <mm:saaja_osakond>
  osakonna nimi, kus vastav isik peaks töötama
 </mm:saaja_osakond>
 <mm:seotud_dhl_id>
  Seotud dokumendi DVK ID.
 </mm:seotud_dhl_id>
 <mm:sisu_id>
  Põhifaili ID, kui SignedDoc konteineris saadetakse mitu faili.
  Vajalik selleks, et saaks eristada põhifaili ja lisafaile.
 </mm:sisu_id>
<!--
  mistahes muud väljad, mida dokumendi DVK-sse lisav
  tarkvara soovib metainfosse lisada. Nende väljade kodeeringu
  viis on erinev, kui eelmistel, DVK jaoks tähendust
  omavatel väljadel.
-->

  <mm:saatja_defineeritud mm:valjanimi="Mingi nimi">
   Mingi väärtus
  </mm:saatja_defineeritud>

    <mm:test>
Näitab, kas tegu on testsõnumiga
</mm:test>
    <mm:dokument_liik>
Dokumendi liik (kiri, õigusakt, arve jne.)
</mm:dokument_liik>
    <mm:dokument_keel>
Keel, milles dokument on koostatud
</mm:dokument_keel>
    <mm:dokument_pealkiri>
Dokumendi pealkiri
</mm:dokument_pealkiri>
<mm:versioon_number>
Dokumendi versiooninumber
</mm:versioon_number>
<mm:dokument_guid>
Dokumendi globaalselt unikaalne identifikaator
</mm:dokument_guid>
<mm:dokument_viit>
Dokumendi viit. Dokumendi registreerimisnumber dokumendihaldussüsteemis(DHS) (Nt. "1.2/4-6")
</mm:dokument_viit>
<mm:kuupaev_registreerimine>
Dokumendi DHS-is registreerimise kuupäev ja kellaaeg
</mm:kuupaev_registreerimine>
<mm:kuupaev_saatmine>
Dokumendi DHS-ist väljasaatmise kuupäev ja kellaaeg
</mm:kuupaev_saatmine>
<mm:tahtaeg>
Dokumendiga seotud toimingu täitmise tähtaeg. Nt. kirja puhul vastamise tähtaeg
</mm:tahtaeg>
<mm:saatja_kontekst>
    <mm:seosviit>
Dokumendi registreerimisnumber algse dokumendi saatja süsteemis
</mm:seosviit>
    <mm:kuupaev_saatja_registreerimine>
Dokumendi registreerimise kuupäev ja kellaaeg algse dokumendi saatja süsteemis
</mm:kuupaev_saatja_registreerimine>
    <mm:dokument_saatja_guid>
Algse dokumendi GUID
</mm:dokument_saatja_guid>
</mm:saatja_kontekst>

    <mm:ipr>
        <mm:ipr_tahtaeg>
Intellektuaalse omandi lõpptähtaeg
</mm:ipr_tahtaeg>
        <mm:ipr_omanik>
Intellektuaalse omandi omaja nimetus
</mm:ipr_omanik>
        <mm:reprodutseerimine_keelatud>
Näitab, kas dokumendi sisu on keelatud taasesitada
</mm:reprodutseerimine_keelatud>
</mm:ipr>

<mm:juurdepaas_piirang>
    <mm:piirang>
Juurdepääsupiirangu kirjeldus
</mm:piirang>
<mm:piirang_algus>
Juurdepääsupiirangu algus
</mm:piirang_algus>
<mm:piirang_lopp>
Juurdepääsupiirangu lõpp
</mm:piirang_lopp>
<mm:piirang_alus>
Juurdepääsupiirangu alus
</mm:piirang_alus>
</mm:juurdepaas_piirang>

<mm:koostajad>
    <mm:koostaja>
        <mm:eesnimi>
Dokumendi koostaja eesnimi
</mm:eesnimi>
        <mm:perenimi>
Dokumendi koostaja perenimi
</mm:perenimi>
        <mm:ametinimetus>
Dokumendi koostaja ametinimetus
</mm:ametinimetus>
        <mm:epost>
Dokumendi koostaja e-posti aadress
</mm:epost>
        <mm:telefon>
Dokumendi koostaja kontakttelefon
</mm:telefon>
</mm:koostaja>
…
<mm:koostaja/>
</mm:koostajad>

</dhl:metainfo>
```


###Transport blokk dokumendis

Transport blokk sisaldab dokumendi edasisaatmiseks kriitilist vajalikku infot. Blokk on kohustuslik, kui dokument on mõeldud edasisaatmiseks. Täpsemalt vaata dokumentide logistika peatükist ja dhl.xsd schemast edasises. Ka siin sõltub bloki struktuur kasutatavast DVK konteineri versioonist. **Versioon 1** puhul on kasutusel järgmine struktuur:
```xml
<dhl:transport xmlns:dhl="http://www.riik.ee/schemas/dhl">
  <!-- üks saatja info blokk -->
 <dhl:saatja>
   <dhl:regnr>Saatja asutuse registreerimisnumber<dhl:regnr>
   <dhl:isikukood>Saatja isikukood<dhl:isikukood>
   <dhl:epost>Saatja eposti aadress<dhl:epost>
   <dhl:nimi>Saatja nimi<dhl:nimi>
   <dhl:asutuse_nimi>Saatja asutuse nimi<dhl:asutuse_nimi>
   <dhl:ametikoha_kood>Saatja ametikoha kood (DVK ID)<dhl:ametikoha_kood>
   <dhl:ametikoha_nimetus>Saatja ametikoha nimetus<dhl:ametikoha_nimetus>
   <dhl:allyksuse_kood>Saatja allüksuse kood (DVK ID)<dhl:allyksuse_kood>
   <dhl:allyksuse_nimetus>Saatja allüksuse nimetus<dhl:allyksuse_nimetus>
   <dhl:osakonna_kood>Saatja struktuuriüksuse asutusesisene kood<dhl:osakonna_kood>
   <dhl:osakonna_nimi> Saatja struktuuriüksuse nimi<dhl:osakonna_nimi>
 </dhl:saatja>
    
 <!-- piiramatu arv saaja info blokke -->
 <dhl:saaja>
   <dhl:regnr>Saaja asutuse registreerimisnumber<dhl:regnr>
   <dhl:isikukood>Saatja isikukood<dhl:isikukood>
   <dhl:epost>Saaja eposti aadress<dhl:epost>
   <dhl:nimi>Saaja nimi<dhl:nimi>
   <dhl:asutuse_nimi>Saaja asutuse nimi<dhl:asutuse_nimi>
   <dhl:ametikoha_kood>Saaja ametikoha kood (DVK ID)<dhl:ametikoha_kood>
   <dhl:ametikoha_nimetus>Saaja ametikoha nimetus<dhl:ametikoha_nimetus>
   <dhl:allyksuse_kood>Saaja allüksuse kood (DVK ID)<dhl:allyksuse_kood>
   <dhl:allyksuse_nimetus>Saaja allüksuse nimetus<dhl:allyksuse_nimetus>
   <dhl:osakonna_kood>Saaja struktuuriüksuse asutusesisene kood<dhl:osakonna_kood>
   <dhl:osakonna_nimi> Saaja struktuuriüksuse nimi<dhl:osakonna_nimi>
 </dhl:saaja>
   ....
 <dhl:saaja/>

 <!-- kuni üks vahendaja info blokk -->
 <dhl:vahendaja>
   <dhl:regnr>Vahendaja asutuse registreerimisnumber<dhl:regnr>
   <dhl:isikukood>Vahendaja isikukood<dhl:isikukood>
   <dhl:epost>Vahendaja eposti aadress<dhl:epost>
   <dhl:nimi>Vahendaja nimi<dhl:nimi>
   <dhl:asutuse_nimi>Vahendaja asutuse nimi<dhl:asutuse_nimi>
   <dhl:ametikoha_kood>Vahendaja ametikoha kood (DVK ID)<dhl:ametikoha_kood>
   <dhl:ametikoha_nimetus>Vahendaja ametikoha nimetus<dhl:ametikoha_nimetus>
   <dhl:allyksuse_kood>Vahendaja allüksuse kood (DVK ID)<dhl:allyksuse_kood>
   <dhl:allyksuse_nimetus>vahendaja allüksuse nimetus<dhl:allyksuse_nimetus>
   <dhl:osakonna_kood>Vahendaja struktuuriüksuse asutusesisene kood<dhl:osakonna_kood>
   <dhl:osakonna_nimi>Vahendaja struktuuriüksuse nimi<dhl:osakonna_nimi>
 </dhl:vahendaja>
</dhl:transport>
```
###DVK konteineri versioon 2 puhul on kasutusel järgmine struktuur:

```xml
<dhl:transport xmlns:dhl="http://www.riik.ee/schemas/dhl/2010/2">
  <!-- üks saatja info blokk -->
 <dhl:saatja>
     <dhl:teadmiseks>
Näitab, kas antud dokument on adressaadile teadmiseks või täitmiseks
 <dhl:teadmiseks>
   <dhl:regnr>Saatja asutuse registreerimisnumber<dhl:regnr>
   <dhl:isikukood>Saatja isikukood<dhl:isikukood>
   <dhl:epost>Saatja eposti aadress<dhl:epost>
   <dhl:nimi>Saatja nimi<dhl:nimi>
   <dhl:asutuse_nimi>Saatja asutuse nimi<dhl:asutuse_nimi>
   <dhl:ametikoha_lyhinimetus>Saatja ametikoha lühinimetus<dhl:ametikoha_kood>
   <dhl:ametikoha_nimetus>Saatja ametikoha nimetus<dhl:ametikoha_nimetus>
   <dhl:allyksuse_lyhinimetus>Saatja allüksuse lühinimetus<dhl:allyksuse_kood>
   <dhl:allyksuse_nimetus>Saatja allüksuse nimetus<dhl:allyksuse_nimetus>
</dhl:saatja>
    
 <!-- piiramatu arv saaja info blokke -->
 <dhl:saaja>
   <dhl:teadmiseks>
Näitab, kas antud dokument on adressaadile teadmiseks või täitmiseks
 <dhl:teadmiseks>
   <dhl:regnr>Saaja asutuse registreerimisnumber<dhl:regnr>
   <dhl:isikukood>Saatja isikukood<dhl:isikukood>
   <dhl:epost>Saaja eposti aadress<dhl:epost>
   <dhl:nimi>Saaja nimi<dhl:nimi>
   <dhl:asutuse_nimi>Saaja asutuse nimi<dhl:asutuse_nimi>
   <dhl:ametikoha_lyhinimetus>Saaja ametikoha lühinimetus<dhl:ametikoha_kood>
   <dhl:ametikoha_nimetus>Saaja ametikoha nimetus<dhl:ametikoha_nimetus>
   <dhl:allyksuse_lyhinimetus>Saaja allüksuse lühinimetus<dhl:allyksuse_kood>
   <dhl:allyksuse_nimetus>Saaja allüksuse nimetus<dhl:allyksuse_nimetus>
 </dhl:saaja>
   ....
 <dhl:saaja/>

 <!-- kuni üks vahendaja info blokk -->
 <dhl:vahendaja>
   <dhl:teadmiseks>
Näitab, kas antud dokument on adressaadile teadmiseks või täitmiseks
 <dhl:teadmiseks>
   <dhl:regnr>Vahendaja asutuse registreerimisnumber<dhl:regnr>
   <dhl:isikukood>Vahendaja isikukood<dhl:isikukood>
   <dhl:epost>Vahendaja eposti aadress<dhl:epost>
   <dhl:nimi>Vahendaja nimi<dhl:nimi>
   <dhl:asutuse_nimi>Vahendaja asutuse nimi<dhl:asutuse_nimi>
   <dhl:ametikoha_lyhinimetus>
     Vahendaja ametikoha lühinimetus
   <dhl:ametikoha_kood>
   <dhl:ametikoha_nimetus>Vahendaja ametikoha nimetus<dhl:ametikoha_nimetus>
   <dhl:allyksuse_lyhinimetus>
     Vahendaja allüksuse lühinimetus
   <dhl:allyksuse_kood>
   <dhl:allyksuse_nimetus>vahendaja allüksuse nimetus<dhl:allyksuse_nimetus>
</dhl:vahendaja>
</dhl:transport>
```
###Ajalugu blokk dokumendis

Ajalugu blokk sisaldab kas täielikke või osalisi koopiaid dokumendi varasematest metainfo-, transport- ja metaxml blokkidest. Ajalugu blokki lisab seniste vastavate blokkide koopiaid dokumenti edasisaatev rakendus, lisades kas tervikblokid või nende osad ajalugu bloki lõppu:

```xml
<dhl:ajalugu xmlns:dhl="http://www.riik.ee/schemas/dhl">
   <dhl:metainfo/>
   <dhl:transport/>
   <dhl:metaxml/>
      ...
   <dhl:metainfo/>
   <dhl:transport/>
   <dhl:metaxml/>
<dhl:ajalugu/>
```
###Metaxml blokk dokumendis
Metaxml bloki sisuks on dokumendi liigist sõltuva struktuuriga metaandmed. Soovituslik oleks igal konkreetsel juhul viidata metaandmete struktuuri kirjeldavale XML skeemile (schema), kasutades selleks xmlns ja schemaLocation atribuute. Vaikimisi eeldatakse, et metaxml blokis sisalduvad andmed vastavad Riigikantselei poolt fikseeritud kirja metaandmete vormingule: (http://www.riik.ee/schemas/dhl/rkel\_letter.xsd)

```xml
<dhl:metaxml xmlns="http://www.riik.ee/schemas/dhl/rkel_letter"
  schemaLocation="http://www.riik.ee/schemas/dhl/rkel_letter
  http://www.riik.ee/schemas/dhl/rkel_letter.xsd">

  <!—- Järgnevad dokumendi liigist sõltuva struktuuriga metaandmed.
     Soovituslik oleks igal konkreetsel juhul viidata metaandmete struktuuri
     kirjeldavale XML skeemile (schema), kasutades selleks xmlns ja schemaLocation
     atribuute. Vaikimisi eeldatakse, et metaxml blokis sisalduvad andmed vastavad
     Riigikantselei poolt fikseeritud kirja metaandmete vormingule:
     (http://www.riik.ee/schemas/dhl/rkel_letter.xsd)
  -->

<dhl:metaxml/>
```
###Taustinfoks:DigiDoci konteiner
Selline on DigiDoc konteineri üldstruktuur:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<SignedDoc format="DIGIDOC-XML" version="1.3"
           xmlns="http://www.sk.ee/DigiDoc/v1.3.0#"                     
    <!-- algandmefailid -->
    <DataFile />
    <!-- kliendi allkirjad koos kehtivuskinnitustega -->
    <Signature />
</SignedDoc>
```

DigiDoc fail sisaldab ühe või enama algandmefaili või viite väliselt salvestatud failile.

Iga faili kohta tehakse kirje `<DataFile>`, mis omab järgmisi atribuute:

Id - faili sisemine unikaalne tunnus.
    Andmefailide tunnused algavad sümboliga 'D', millele järgneb faili järjekorranumber.

Filename - faili tegelik (väline) nimi ilma teekonnata.

ContentType - dokumendi salvestamise meetod (DETATCHED, EMBEDDED\_BASE64 või EMBEDDED)

EMBEDDED - faili andmed on sisestatud algkujul antud kirjes.
Kasutatav vaid XML kujul algandmete jaoks. Tähelepanu tuleb osutada sellel, et algandmete XML fail ei sisaldaks XML päist (`<?xml ... ?>`) ega DTD-d. Siin kirjeldatud XML elemendid ei ole keelatud. Võmalik on ühe faili sisse salvestada algkujul teist DigiDoc faili.

EMBEDDED\_BASE64 - faili andmed on sisestatud Base64 kujul antud kirjes.

DETATCHED - algandmed sisalduvad failis, mille nimi on salvestatud atribuudis Filename.

MimeType - algandmete andmetüüp.

Size - tegeliku algandmefaili suurus baitides.

DigestType - algandmefaili räsikoodi tüüp. Esialgu toetatakse vaid sha1 tüüpi. Nõutud vaid DETATCHED tüüpi faili puhul.

DigestValue - algandmefaili räsikoodi väärtus Base64 kujul. Räsi arvutatakse algandmete üle nende originaalkujul. Nõutud vaid DETATCHED tüüpi faili puhul.

Suvaline hulk muid atribuute (metaandmed) kujul `<nimi>`="`<väärtus>`".

xmlns - peab kasutama SignedDoc namespacet: http://www.sk.ee/DigiDoc/v1.3.0\#.


###Taustinfoks:`<failid>` konteiner
DVK konteineri versioon 2 kasutab failide edastamiseks <SignedDoc> (DigiDoc) konteineri asemel `<failid>` konteinerit, mille struktuur on järgmine:

```xml
<dhl:failid xmlns=“http://www.riik.ee/schemas/dhl/2010/2“>
    <dhl:kokku>Failide arv</dhl:kokku>
    <dhl:fail>
        <dhl:jrknr>Faili järjekorranumber</dhl:jrknr>
        <dhl:fail_pealkiri>Faili pealkiri</dhl:fail_pealkiri>
        <dhl:fail_suurus>Faili suurus baitides</dhl:fail_suurus>
        <dhl:fail_tyyp>Faili MIME tüüp</dhl:fail_tyyp>
        <dhl:fail_nimi>Faili nimi</dhl:fail_nimi>
        <dhl:zip_base64_sisu>
ZIP-itud ja Base64 kodeeringusse pandud faili sisu
</dhl:zip_base64_sisu>
        <dhl:krypteering>Näitab, kas fail on krüpteeritud</dhl:krypteering>
<dhl:pohi_dokument>Näitab, kas tegemist on põhifailiga</dhl:pohi_dokument>
<dhl:pohi_dokument_konteineris>
Kui tegemist on konteinerfailiga (BDOC, DDOC, ZIP), siis annab põhifaili nime
</dhl:pohi_dokument_konteineris>
    </dhl:fail>
</dhl:failid>
```

##Dokumentide logistika

###Üldist
Kahe erineva asutuse dokumendihaldussüsteemide (DHS) vaheline automaatne elektrooniline dokumendivahetus realiseeritakse üle X-tee dokumendivahetuskeskuse (DVK) kaasabil. DHS-ile paistab DVK kätte kui X-tee infrastruktuuris asuv andmekogu ning ka sellega suhtlus realiseeritakse standardsel viisil – DHS-i juurde realiseeritakse X-tee adapterserver, mis suhtleb läbi asutuse turvaserveri DVK-ga.

###Tööskeem

####DVK loogiline ülesehitus
Dokumentide logistika teenuste seisukohast omab DVK järgmist loogilist ülesehitust:

-   Kõik DVK-d üle X-Tee kasutavad asutused on ära kirjeldatud suhtluspartneritena. Iga suhtluspartner omab DVK-s kontot, mille kaudu saab teistele asutustele dokumente saata ja endale saadetud dokumente vastu võtta.
-   Dokumente on DVK poolel võimalik vastavalt funktsioonile või dokumendiliigile paigutada kaustadesse. Iga dokument asub DVK-s mingis kaustas. Kui dokumendi saatmisel kausta ei määrata, paigutatakse dokument DVK-s nn. juurkausta.
-   Iga DVK-sse saadetava dokumendi puhul määratakse ära dokumendi saatja ning ühe või mitme adressaadi andmed. Adressaatide andmete alusel otsustatakse, millistele asutustele antud dokumenti üleslaadimiseks pakutakse.
-   DVK-sse saabumisel fikseeritakse iga adressaadi kohta dokumendi olekuks „saatmisel”. Kui adressaat on dokumendi DVK-st vastu võtnud ja dokumendi kättesaamist kinnitanud, saab dokument antud adressaadi seisukohast olekuks „saadetud”. Iga dokumendi kohta hoitakse DVK-s ka koondolekut. Kui dokument ootab saatmist vähemalt ühele adressaadile, on dokumendi koondolek „saatmisel”. Kui dokument on kõigi adressaatide poolt vastu võetud, saab dokumendi koondolekuks „saadetud”.

####Kaustade kasutamine
Juhul, kui tekib vajadus vahetada asutuste vahel mitut erinevat liiki dokumente või kui samas asutuses vahetavad DVK kaudu dokumente mitu erinevat rakendust, on otstarbekas kasutada dokumentide organiseerimiseks DVK poolel kaustade funktsionaalsust. S.t. näiteks e-arveid vahetavad asutused kausta /FINANTS kaudu ja kirju kausta /KIRJAD kaudu.

Vaikimisi võiks DVK rakendus sisaldada järgmisi kaustu:

-   FINANTS
-   PKD
-   KIRJAVAHETUS
-   OIGUSAKT
-   EVORMID

##### Kaustade kasutamine testimisel

Testimiseks on loodud eraldi testkeskkond. Toodangukeskkonnas tohib teostada vaid minimaalset testimist, kindlustamaks et DHS on suuteline DVK-ga andmeid vahetama.
Toodangukeskkonnas testimiseks tuleb test dokumente edastada kausta TEST. Selleks, et test dokumendid ei segaks asutuse kirjavahetust on väljaspool testperioodi soovitatav pärida dokumente kaustast „/“, mis tagastab vaid juurkataloogis olevad dokumendid.

##### Kaustade kasutamine e-vormide andmevahetuses

E-vorme kasutaval asutusel on võimalik vormid kätte saada ja neile vastata DVK kaudu.

E-vorm edastatakse e-vormide rakenduse (edaspidi EV) poolt asutusele ja asutuselt EV-le DVK dokumendikonteineris oleva failina "evorm.xml". Samas konteineris on ka vorm HTML vormingus.

Kui saadetava e-vormi adressaadiks olev asutus on DVK andmetel võimeline dokumente vastu võtma DVK vahendusel, siis toimetatakse vorm DVK kausta "EVORMID" või EV rakenduses vormi jaoks määratud kausta. Kuni vormi kättesaamiseni adressaadi poolt on vormi võimalik alla laadida ka eCommunity andmevahetusliidese kaudu, kuid allalaadimine eCommunity liidese kaudu ei tühista saatmist DVK kaudu.

Kui asutusel töötleb sama dokumendihaldussüsteem (edaspidi: DHS) kõiki DVK kaudu saabuvaid dokumente (mh vorme ja kirju), siis peaks DVK päringul jätma kausta nime määramata. Sellisel juhul laaditakse asutuse DHS –i kõik DVK –s asuvad dokumendid (sh vormid).

Kui asutusel töötleb vorme ja muid DVK dokumente erinev IS, peaks e-vorme töötlev rakendus dokumentide DVK –st allalaadimisel määrama kaustaks "EVORMID" (või EV rakenduses määratud kausta). Muude dokumentidega (peale e-vormide) tegelev asutuse DHS võib:

-   laadida DVK –st alla laadida kõik dokumendid (jättes määramata kausta), märkides neist kättesaaduks ainult need, mida ei töötle vormidega tegelev IS;
-   laadida dokumendid alla DVK juurkaustast või DVK päringu receiveDocuments parameetris "kaust" esitatud loetelus ette antud kaustadest (v.a vorme sisaldav kaust).

Vormile vastuse saatmisel peaks vastus olema adresseeritud samasse kausta, kuhu oli paigutatud algne EV rakenduse kasutaja poolt saadetud dokument. DVK-st allalaaditava dokumendi kaust on määratud elemendiga "ma:dhl\_kaust".

####Dokumentide edastamine

PILT!

Dokumentide edastamiseks teis(t)ele asutus(t)ele käivitab DHS päringu *dhl.sendDocuments*. Päringu kehasse paigutatakse massiivina kõik edastamist nõudvad dokumendid. Dokumendid peavad olema esitatud „dhlDokumentType“ XML tüübile vastavas formaadis (vt punkti „Dokumentide formaat“) ning omama elemendi „Transport“ all järgmisi kohustuslikke elemente:

-   saatja – element määrab dokumenti väljasaatva asutuse (üldjuhul sama, mis dokumenti edastav asutus)
-   saaja – üks kuni mitu elementi määravad asutused, millele tuleb antud dokument edastada. Kui saaja aadressinfos on määratud väli „epost“, siis edastatakse see dokument näidatud e-posti aadressile. Kui aadressinfos on määratud väli „regnr“ ning sellise registreerimisnumbriga asutus on DVK-s suhtluspartnerina kirjeldatud, toimub dokumendi üleandmine DVK siseselt.

Kui DVK server tuvastab, et mõne adressaadi jaoks tuleb dokument edastada mõnda teise DVK serverisse, siis lisab DVK server automaatselt dokumendi konteineri „transport“ elemendi alla elemendi:

-   vahendaja – element määrab selle asutuse andmed, mille nimel dokument ühest DVK serverist teise edastati.

DVK poolel saavad dokumendid staatuse „saatmisel” ja jäävad ootama *dhl.receiveDocuments* päringut. Päring tagastab vea, kui edastatud dokumentide struktuur sisaldab olulisi vigu. Päring ei tagasta viga juhul, kui mingite dokumentide adressaadile edastamine ei õnnestunud – edastamine toimub asünkroonselt. Edastamise õnnestumist saab kontrollida päringuga *dhl.getSendStatus*.
Antud päringuga saab dokumente salvestada ka DVK-sse selliselt, et edastamist saaja(te)le ei toimu. Sellisel juhul peab olema päringu parameeter „kaust“ olema väärtustatud selle kaustateega, kuhu soovitakse dokumendid salvestada ning „transport“ element ei tohi sisaldada ühtegi elementi „saaja“.
Päringuga on võimalik edastada ka dokumente, mis asuvad DVK dokumendikontol. Sellisel juhul tuleb tegeliku dokumendi asemel esitada dokumendi kehas element „ref“ vajaliku atribuudi väärtusega (kas atribuut „dhl\_id“ dokumendi ID väärtusega või siis „dhl\_taisnimi“ DVK kaustapuu dokumendi täispika nimega). Sealjuures on lubatud esitada ka element „Transport“, mis kirjutab dokumendil esineda võiva „Transport“ elemendi üle.
Päring tagastab resultaadina massiivi, mille vastavatel positsioonidel on edastatud dokumentide DVK seesmised ID-id.

####Edastatud dokumentide staatuse kontroll
Päringuga *dhl.getSendStatus* küsitakse päringu *dhl.sendDocuments* abil edastatud dokumentide olekuinfot. Päringu kehas esitatakse massiiv dokumentide ID-idest(DVK konteineri versioonis 2 on võimalik leida dokumendi staatus ka dokumendi GUID abil), mille olekut soovitakse teada saada. Päring tagastab massiivi, mille elementideks on dokumentide saatmisinfo. Saatmisinfo sisaldab iga saaja kohta elementi „edastus“, mille alamelementide tähendused on:

- „saaja“ element on samasuguse sisuga, nagu dokumendi saaja element oli algselt määratud.
- „saadud“ väärtuseks on kuupäev ja kellaaeg, mil DVK dokumendi saatjalt kätte sai.
- „meetod“ sisaldab edastusmeetodi väärtust: kas „xtee“ või „epost“.
- „edastatud“ väärtuseks on kuupäev ja kellaaeg, mil DVK kas saatis dokumendi e-posti aadressile või kui adressaat märkis dokumendi vastuvõetuks.
- „loetud“ väärtuseks on kuupäev ja kellaeg, mil teise osapoole DHS märkis päringuga *dhl.markDocumentsReceived* kättesaaduks. Saaja tohib dokumendi märkiga kättesaaduks siis, kui dokument on ametnikule nähtavaks tehtud. Dokumendi saabumine süsteemi ei ole võrdne dokumendi kätte saamisega vastuvõtja poolel. Juhul, kui dokument saabub DHS süsteemi, kuid selle kuvamine kasutajale ei ole võimalik, tuleb dokumendi staatuseks määrata „katkestatud“ (e-posti teel edastuse korral jääb see väli väärtustamata).
- „fault“ element on väärtustatud ainult juhul, kui edastamise käigus juhtus viga
- „staatus“ element näitab, mis **seisus antud saajale edastus on**. Võimalikud väärtused on:
    - „saatmisel“ – dokumenti üritatakse veel antud saajale edastada
    - „saadetud“ – dokument sai edukalt antud saajale saadetud
    - „katkestatud“ – dokumenti ei õnnestunud antud saajale saata.
- „vastuvotja\_staatus\_id“ element sisaldab adressaadi poolel dokumendile antud staatuse identifikaatorit. Element on väärtustatud ainult juhul, kui adressaat on omalt poolt dokumendile antud staatuse DVK-le edastanud.
- „metaxml” element sisaldab vaba struktuuriga XML- või tekstiandmeid vastuvõetud dokumenti puudutavate andmete saatmiseks dokumendi saatjale.

Saatmisinfo kirje sisaldab ka elemendi „olek“, **mille väärtus määrab dokumendi edastuse koondoleku**. Võimalikud väärtused on:

- „saatmisel“ – leidub vähemalt üks saaja, kellele veel üritatakse antud dokumenti saata.
- „saadetud“ – dokument õnnestus kõigile saajatele edastada.
- „katkestatud“ – vähemalt ühele saajale ei õnnestunud dokumenti edastada.

####Dokumentide vastuvõtt
DHS-i poolne teiste asutuste poolt antud asutusele saadetud dokumentide vastuvõtt toimub päringu *dhl.receiveDocuments* abil. Päring tagastab kõik DVK-s antud asutusele teiste asutuste poolt edastatud dokumendid. Päringule saab elemendi „arv“ abil määrata piirangu, mitu dokumenti maksimaalselt tohib vastuses tagastada. Lisaks saab elemendi „kaust“ abil määrata kausta(d), kust dokumente loetakse.
Päring tagastab loetud dokumentide massiivi.
Peale edukat dokumentide vastuvõttu peab DHS käivitama päringu *dhl.markDocumentsReceived*, mille abil signaliseeritakse DVK-le, et näidatud dokumendid said edukalt alla laetud. Sellest päringust eksisteerib 3 versiooni:

- Päringu esimene versioon (v1) nõuab elemendi „dokumendid“ väärtuseks loetud dokumentide ID-ide massiivi.
- Päringu teine versioon (v2) nõuab elemendi „dokumendid“ väärtuseks loetud dokumentide kohta massiivi järgmise struktuuriga andmeelementidest:
    - „dhl\_id” element sisaldab loetud dokumendi DVK ID-d.
    - „vastuvotja\_staatus\_id” element sisaldab vastuvõtja poolel dokumendile omistatud staatuse identifikaatorit.
    - „fault” element sisaldab andmeid vastuvõtja poolel ilmnenud vea kohta.
    - „metaxml” vaba struktuuriga XML- või tekstiandmed vastuvõetud dokumenti puudutavate andmete saatmiseks dokumendi saatjale.
-   Päringu kolmandas versioonis
    Päring märgib esitatud ID-dele vastavad dokumendid DVK-s päringu teinud asutuse poolt vastuvõetuks. Kui päringul on väärtustatud parameeter „kaust“, siis toimub märgitakse vastuvõetuks ainult märgitud kaustas asuvad dokumendid.

Päring tagastab oma kehas väärtuse „OK“.

##X-Tee päringute kirjeldused

###sendDocuments

###sendDocuments.v1

Päringu nimi: dhl.sendDocuments.v1
Sisendi keha: Struct
base64Binary dokumendid
string kaust
Väljundi keha: base64Binary

Päring tuleb realiseerida vastavalt X-Tee dokumentatsioonis MIME manuseid sisaldavate teadete realiseerimise skeemile.
Element „dokumendid“ on base64 kodeeringus documentsArrayType tüüpi massiiv DVK-sse saadetavatest dokumentidest.
Element „kaust“ määrab ära kataloogitee, kuhu tuleb dokumendid paigutada. Element võib ka puududa, sellisel juhul on kataloogiks üldine juurkataloog „/”. Kui elemendiga „kaust” antud kausta ei eksisteeri DVK-s, siis luuakse see automaatselt.
Väljundi kehaks on base64 kodeeringus documentRefsArrayType tüüpi massiiv, mis sisaldab DVK poolt dokumentidele omistatud unikaalseid ID-e.
Dokumentide saatmisel sendDocuments päringuga teostatakse DVK konteineri ning saadetavate XML, DDOC ja BDOC failide valideerimine. Täpsem info failide valideerimise kohta asub käesoleva dokumendi peatükis „[Edastatavate dokumentide valideerimine DVK serveris](#edastatavate-dokumentide-valideerimine-dvk-serveris)“.

####Näide

#####Päring

```
POST /cgi-bin/consumer_proxy HTTP/1.0
Content-Type: multipart/related; type=text/xml;
    boundary="=_b5a8d09eeeb161be29def84633d6f6fc"
SOAPAction: ""

--=_b5a8d09eeeb161be29def84633d6f6fc
Content-Type:text/xml; charset="UTF-8"
Content-Transfer-Encoding:8bit

<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope
  xmlns:SOAPENV="http://schemas.xmlsoap.org/soap/envelope/"
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"
  xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"
  xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"
  SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
<SOAP-ENV:Header>
  <xtee:asutus xsi:type="xsd:string">12345678</xtee:asutus>
  <xtee:andmekogu xsi:type="xsd:string">dhl</xtee:andmekogu>
  <xtee:isikukood xsi:type="xsd:string">EE12345678901</xtee:isikukood>
  <xtee:id xsi:type="xsd:string">6cae248568b3db7e97ff784673a4d38c5906bee0</xtee:id>
  <xtee:nimi xsi:type="xsd:string">dhl.sendDocuments.v1</xtee:nimi>
  <xtee:toimik xsi:type="xsd:string"></xtee:toimik>
  <xtee:amet xsi:type="xsd:string"></xtee:amet>
</SOAP-ENV:Header>
<SOAP-ENV:Body>
  <dhl:sendDocuments>
    <keha>
      <dokumendid href="cid:b8fdc418df27ba3095a2d21b7be6d802"/>
      <kaust/>
    </keha>
  </dhl:sendDocuments>
</SOAP-ENV:Body>
</SOAP-ENV:Envelope>

--=_b5a8d09eeeb161be29def84633d6f6fc
Content-Disposition:php5hmCiX
Content-Type:{http://www.w3.org/2001/XMLSchema}base64Binary
Content-Transfer-Encoding:base64
Content-Encoding: gzip
Content-ID:<b8fdc418df27ba3095a2d21b7be6d802>

a29ybmkJa3cNCmtvcnNpa2EJY28NCmtyZWVrYQllbA0Ka3JpaQn1ZA0Ka3Vt9Wtp
…
dg0KbmVwYWxpCW5lDQpuaXZoaQnkdA0KbmphbmT+YQlueQ0Kbm9nYWkJ9ncNCg==
--=_b5a8d09eeeb161be29def84633d6f6fc
```

##### Päringu vastus

```xml
HTTP/1.1 200 OK
Content-Type: multipart/related; type=text/xml;
    boundary="=_9d665408f43f4698f71029c2df2b834e"

--=_9d665408f43f4698f71029c2df2b834e
Content-Type:text/xml; charset="UTF-8"
Content-Transfer-Encoding:8bit

<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAPENV="
  http://schemas.xmlsoap.org/soap/envelope/"
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"
  xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"
  xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"
  SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
<SOAP-ENV:Header>
<xtee:asutus xsi:type="xsd:string">12345678</xtee:asutus>
<xtee:andmekogu xsi:type="xsd:string">dhl</xtee:andmekogu>
<xtee:isikukood xsi:type="xsd:string">EE12345678901</xtee:isikukood>
<xtee:id
xsi:type="xsd:string">6cae248568b3db7e97ff784673a4d38c5906bee0</xtee:id>
<xtee:nimi xsi:type="xsd:string">dhl.sendDocuments.v1</xtee:nimi>
<xtee:toimik xsi:type="xsd:string"></xtee:toimik>
<xtee:amet xsi:type="xsd:string"></xtee:amet>
</SOAP-ENV:Header>
<SOAP-ENV:Body>
  <dhl:sendDocumentsResponse>
    <paring>
      <dokumendid xsi:type="xsd:string">52ed0ffbf27fc34759dce05d0e7bed2302876cec</dokumendid>
      <kaust/>
    </paring>
    <keha href="cid:793340a8216da081f3d785bcc74c0f74"/>
  </dhl:sendDocumentsResponse>
</SOAP-ENV:Body>
</SOAP-ENV:Envelope>

--=_9d665408f43f4698f71029c2df2b834e
Content-Type:{http://www.w3.org/2001/XMLSchema}base64Binary
Content-Transfer-Encoding:base64
Content-Encoding: gzip
Content-ID:<793340a8216da081f3d785bcc74c0f74>

VMO1ZWxpbmUgdGFsdiB2w7VpYiBzYWFidWRhIGhpbGplbSBrdWkgdGF2YWxpc2Vs
dC4K

--=_9d665408f43f4698f71029c2df2b834e
```


##### Päringu „dokumendid“ elemendi sisu
Elemendi „dokumendid“ sisu kodeerimata kujul on:
```xml
<dokument xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl">
    <metainfo/>
    <transport>
        <saatja>
            <regnr>00000000</regnr>
            <nimi>String</nimi>
            <osakonna_kood>String</osakonna_kood>
            <osakonna_nimi>String</osakonna_nimi>
        </saatja>
        <saaja>
            <regnr>00000000</regnr>
            <nimi>String</nimi>
            <osakonna_kood>String</osakonna_kood>
            <osakonna_nimi>String</osakonna_nimi>
        </saaja>
        <saaja>
            <regnr>00000000</regnr>
            <nimi>String</nimi>
            <osakonna_kood>String</osakonna_kood>
            <osakonna_nimi>String</osakonna_nimi>
        </saaja>
    </transport>
    <ajalugu/>
    <metaxml>
        <p>Siin sees võib olla suvaline XML jada</p>
        <item>Mingeid piiraguid elementidele pole</item>
    </metaxml>
    <SignedDoc format="DIGIDOC-XML" version="1.3" xmlns="http://www.sk.ee/DigiDoc/v1.3.0#">
        <DataFile ContentType="EMBEDDED_BASE64" Filename="dhl.xsd" Id="D0" MimeType="text/xml" Size="3075" xmlns="http://www.sk.ee/DigiDoc/v1.3.0#">PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iaXNvLTg4NTktMSI/Pg0KPCEt
</DataFile>
        <DataFile ContentType="EMBEDDED_BASE64" Filename="dhl-aadress.xsd" Id="D1" MimeType="text/xml" Size="2679" xmlns="http://www.sk.ee/DigiDoc/v1.3.0#">PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iaXNvLTg4NTktMSI/Pg0KPCEt
</DataFile>
    </SignedDoc>
</dokument>
```

##### Päringu vastuses „keha“ elemendi sisu

Elemendi „keha“ sisu kodeerimata kujul on: `<dhl_id>54365435</dhl>`

Kui saadeti korraga mitu dokumenti, siis on elemendi „keha“ sisu kodeerimata kujul selline:

```
<dhl_id>54365435</dhl_id>
<dhl_id>54365436</dhl_id>
<dhl_id>54365437</dhl_id>
```

###sendDocuments.v2
Päringu sendDocuments versioon v2 erineb eelmisest versioonist selle poolest, et võimaldab dokumente DVK serverisse saata fragmenteeritud kujul.

Päringu nimi: dhl.sendDocuments.v2
Sisendi keha: Struct
    base64Binary dokumendid
    string kaust
    date sailitustahtaeg
    string edastus\_id
    integer fragment\_nr
    integer fragmente\_kokku
Väljundi keha: base64Binary

Päring tuleb realiseerida vastavalt X-Tee dokumentatsioonis MIME manuseid sisaldavate teadete realiseerimise skeemile.
Element „dokumendid“ on base64 kodeeringus documentsArrayType tüüpi massiiv DVK-sse saadetavatest dokumentidest.
Element „kaust“ määrab ära kataloogitee, kuhu tuleb dokumendid paigutada. Element võib ka puududa, sellisel juhul on kataloogiks üldine juurkataloog „/”. Kui elemendiga „kaust” antud kausta ei eksisteeri DVK-s, siis luuakse see automaatselt.
Element „sailitustahtaeg“ määrab ära, kui kaua DVK server peaks saadetavat dokumenti säilitama, enne kui see kettaruumi vabastamiseks serverist maha kustutatakse.
Element „edastus\_id“ määrab edastussessiooni identifikaatori, kui dokumente saadetakse tükkhaaval. Tükkhaaval saatmisel loeb DVK server kõik sama „edastus\_id“ väärtusega saadetud andmed ühe ja sama dokumendi fragmentideks. Kui andmeid ei saadeta tükkhaaval, siis pole seda parameetrit vaja määrata.
Element „fragment\_nr“ määrab tükkhaaval saadetavate andmete puhul, mitmenda tükiga on antud juhul tegu (loendamine algab 0-st). Kui andmeid ei saadeta tükkhaaval, siis pole seda parameetrit vaja määrata.
Element „fragmente\_kokku“ määrab tükkhaaval saadetavate andmete puhul, mitmeks tükiks on saadetavad andmed jaotatud. Kui andmeid ei saadeta tükkhaaval, siis pole seda parameetrit vaja määrata.
Väljundi kehaks on base64 kodeeringus documentRefsArrayType tüüpi massiiv, mis sisaldab DVK poolt dokumentidele omistatud unikaalseid ID-e.

Dokumentide saatmisel sendDocuments päringuga teostatakse DVK konteineri ning saadetavate XML, DDOC ja BDOC failide valideerimine. Täpsem info failide valideerimise kohta asub käesoleva dokumendi peatükis „[Edastatavate dokumentide valideerimine DVK serveris](#)“.

#### Näide

##### Päring

```
POST /cgi-bin/consumer_proxy HTTP/1.0
Content-Type: multipart/related; type=text/xml;
    boundary="=_b5a8d09eeeb161be29def84633d6f6fc"
SOAPAction: ""

--=_b5a8d09eeeb161be29def84633d6f6fc
Content-Type:text/xml; charset="UTF-8"
Content-Transfer-Encoding:8bit

<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope
  xmlns:SOAPENV="http://schemas.xmlsoap.org/soap/envelope/"
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"
  xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"
  xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"
  SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
<SOAP-ENV:Header>
  <xtee:asutus xsi:type="xsd:string">12345678</xtee:asutus>
  <xtee:andmekogu xsi:type="xsd:string">dhl</xtee:andmekogu>
  <xtee:isikukood xsi:type="xsd:string">EE12345678901</xtee:isikukood>
  <xtee:id xsi:type="xsd:string">6cae248568b3db7e97ff784673a4d38c5906bee0</xtee:id>
  <xtee:nimi xsi:type="xsd:string">dhl.sendDocuments.v2</xtee:nimi>
  <xtee:toimik xsi:type="xsd:string"></xtee:toimik>
  <xtee:amet xsi:type="xsd:string"></xtee:amet>
</SOAP-ENV:Header>
<SOAP-ENV:Body>
  <dhl:sendDocuments>
    <keha>
      <dokumendid href="cid:b8fdc418df27ba3095a2d21b7be6d802"/>
      <kaust/>
      <sailitustahtaeg>2009-01-25T23:59:59</sailitustahtaeg>
      <edastus_id>123456789_1201</edastus_id>
      <fragment_nr>-1</fragment_nr>
      <fragmente_kokku>0</fragmente_kokku>
    </keha>
  </dhl:sendDocuments>
</SOAP-ENV:Body>
</SOAP-ENV:Envelope>

--=_b5a8d09eeeb161be29def84633d6f6fc
Content-Disposition:php5hmCiX
Content-Type:{http://www.w3.org/2001/XMLSchema}base64Binary
Content-Transfer-Encoding:base64
Content-Encoding: gzip
Content-ID:<b8fdc418df27ba3095a2d21b7be6d802>

a29ybmkJa3cNCmtvcnNpa2EJY28NCmtyZWVrYQllbA0Ka3JpaQn1ZA0Ka3Vt9Wtp
…
dg0KbmVwYWxpCW5lDQpuaXZoaQnkdA0KbmphbmT+YQlueQ0Kbm9nYWkJ9ncNCg==
--=_b5a8d09eeeb161be29def84633d6f6fc
```

##### Päringu vastus

```
HTTP/1.1 200 OK
Content-Type: multipart/related; type=text/xml;
    boundary="=_9d665408f43f4698f71029c2df2b834e"

--=_9d665408f43f4698f71029c2df2b834e
Content-Type:text/xml; charset="UTF-8"
Content-Transfer-Encoding:8bit

<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAPENV="
  http://schemas.xmlsoap.org/soap/envelope/"
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"
  xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"
  xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"
  SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
<SOAP-ENV:Header>
<xtee:asutus xsi:type="xsd:string">12345678</xtee:asutus>
<xtee:andmekogu xsi:type="xsd:string">dhl</xtee:andmekogu>
<xtee:isikukood xsi:type="xsd:string">EE12345678901</xtee:isikukood>
<xtee:id
xsi:type="xsd:string">6cae248568b3db7e97ff784673a4d38c5906bee0</xtee:id>
<xtee:nimi xsi:type="xsd:string">dhl.sendDocuments.v1</xtee:nimi>
<xtee:toimik xsi:type="xsd:string"></xtee:toimik>
<xtee:amet xsi:type="xsd:string"></xtee:amet>
</SOAP-ENV:Header>
<SOAP-ENV:Body>
  <dhl:sendDocumentsResponse>
    <paring>
      <dokumendid xsi:type="xsd:string">52ed0ffbf27fc34759dce05d0e7bed2302876cec</dokumendid>
      <kaust/>
      <sailitustahtaeg>2009-01-25T23:59:59</sailitustahtaeg>
      <edastus_id>123456789_1201</edastus_id>
      <fragment_nr>-1</fragment_nr>
      <fragmente_kokku>0</fragmente_kokku>
    </paring>
    <keha href="cid:793340a8216da081f3d785bcc74c0f74"/>
  </dhl:sendDocumentsResponse>
</SOAP-ENV:Body>
</SOAP-ENV:Envelope>

--=_9d665408f43f4698f71029c2df2b834e
Content-Type:{http://www.w3.org/2001/XMLSchema}base64Binary
Content-Transfer-Encoding:base64
Content-Encoding: gzip
Content-ID:<793340a8216da081f3d785bcc74c0f74>

VMO1ZWxpbmUgdGFsdiB2w7VpYiBzYWFidWRhIGhpbGplbSBrdWkgdGF2YWxpc2Vs
dC4K

--=_9d665408f43f4698f71029c2df2b834e
```


##### Päringu „dokumendid“ elemendi sisu

Elemendi „dokumendid“ sisu kodeerimata kujul on:

```
<dokument xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl">
    <metainfo/>
    <transport>
        <saatja>
            <regnr>00000000</regnr>
            <nimi>String</nimi>
            <osakonna_kood>String</osakonna_kood>
            <osakonna_nimi>String</osakonna_nimi>
        </saatja>
        <saaja>
            <regnr>00000000</regnr>
            <nimi>String</nimi>
            <osakonna_kood>String</osakonna_kood>
            <osakonna_nimi>String</osakonna_nimi>
        </saaja>
        <saaja>
            <regnr>00000000</regnr>
            <nimi>String</nimi>
            <osakonna_kood>String</osakonna_kood>
            <osakonna_nimi>String</osakonna_nimi>
        </saaja>
    </transport>
    <ajalugu/>
    <metaxml>
        <p>Siin sees võib olla suvaline XML jada</p>
        <item>Mingeid piiraguid elementidele pole</item>
    </metaxml>
    <SignedDoc format="DIGIDOC-XML" version="1.3" xmlns="http://www.sk.ee/DigiDoc/v1.3.0#">
        <DataFile ContentType="EMBEDDED_BASE64" Filename="dhl.xsd" Id="D0" MimeType="text/xml" Size="3075" xmlns="http://www.sk.ee/DigiDoc/v1.3.0#">PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iaXNvLTg4NTktMSI/Pg0KPCEt
</DataFile>
        <DataFile ContentType="EMBEDDED_BASE64" Filename="dhl-aadress.xsd" Id="D1" MimeType="text/xml" Size="2679" xmlns="http://www.sk.ee/DigiDoc/v1.3.0#">PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iaXNvLTg4NTktMSI/Pg0KPCEt
</DataFile>
    </SignedDoc>
</dokument>
```

##### Päringu vastuses „keha“ elemendi sisu

Elemendi „keha“ sisu kodeerimata kujul on:

```
<dhl_id>54365435</dhl_id>
```

Kui saadeti korraga mitu dokumenti, siis on elemendi „keha“ sisu kodeerimata kujul selline:

```
<dhl_id>54365435</dhl_id>
<dhl_id>54365436</dhl_id>
<dhl_id>54365437</dhl_id>
```

###sendDocuments.v3
Päring erineb versioonist 2 selle poolest, et kasutusele on võetud asutuse ja allüksuse lühinimetused.
Päring tuleb realiseerida vastavalt X-Tee dokumentatsioonis MIME manuseid sisaldavate teadete realiseerimise skeemile.

#### Näide

##### Päring

```
POST /cgi-bin/consumer_proxy HTTP/1.0
Content-Type: multipart/related; type=text/xml;
    boundary="=_b5a8d09eeeb161be29def84633d6f6fc"
SOAPAction: ""

--=_b5a8d09eeeb161be29def84633d6f6fc
Content-Type:text/xml; charset="UTF-8"
Content-Transfer-Encoding:8bit

<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope
  xmlns:SOAPENV="http://schemas.xmlsoap.org/soap/envelope/"
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"
  xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"
  xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"
  SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
<SOAP-ENV:Header>
  <xtee:asutus xsi:type="xsd:string">12345678</xtee:asutus>
  <xtee:andmekogu xsi:type="xsd:string">dhl</xtee:andmekogu>
  <xtee:isikukood xsi:type="xsd:string">EE12345678901</xtee:isikukood>
  <xtee:id xsi:type="xsd:string">6cae248568b3db7e97ff784673a4d38c5906bee0</xtee:id>
  <xtee:nimi xsi:type="xsd:string">dhl.sendDocuments.v2</xtee:nimi>
  <xtee:toimik xsi:type="xsd:string"></xtee:toimik>
  <xtee:amet xsi:type="xsd:string"></xtee:amet>
</SOAP-ENV:Header>
<SOAP-ENV:Body>
  <dhl:sendDocuments>
    <keha>
      <dokumendid href="cid:b8fdc418df27ba3095a2d21b7be6d802"/>
      <kaust/>
      <sailitustahtaeg>2009-01-25T23:59:59</sailitustahtaeg>
      <edastus_id>123456789_1201</edastus_id>
      <fragment_nr>-1</fragment_nr>
      <fragmente_kokku>0</fragmente_kokku>
    </keha>
  </dhl:sendDocuments>
</SOAP-ENV:Body>
</SOAP-ENV:Envelope>

--=_b5a8d09eeeb161be29def84633d6f6fc
Content-Disposition:php5hmCiX
Content-Type:{http://www.w3.org/2001/XMLSchema}base64Binary
Content-Transfer-Encoding:base64
Content-Encoding: gzip
Content-ID:<b8fdc418df27ba3095a2d21b7be6d802>

a29ybmkJa3cNCmtvcnNpa2EJY28NCmtyZWVrYQllbA0Ka3JpaQn1ZA0Ka3Vt9Wtp
…
dg0KbmVwYWxpCW5lDQpuaXZoaQnkdA0KbmphbmT+YQlueQ0Kbm9nYWkJ9ncNCg==
--=_b5a8d09eeeb161be29def84633d6f6fc
```

##### Päringu vastus

```
HTTP/1.1 200 OK
Content-Type: multipart/related; type=text/xml;
    boundary="=_9d665408f43f4698f71029c2df2b834e"

--=_9d665408f43f4698f71029c2df2b834e
Content-Type:text/xml; charset="UTF-8"
Content-Transfer-Encoding:8bit

<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope xmlns:SOAPENV="
  http://schemas.xmlsoap.org/soap/envelope/"
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"
  xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"
  xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"
  SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
<SOAP-ENV:Header>
<xtee:asutus xsi:type="xsd:string">12345678</xtee:asutus>
<xtee:andmekogu xsi:type="xsd:string">dhl</xtee:andmekogu>
<xtee:isikukood xsi:type="xsd:string">EE12345678901</xtee:isikukood>
<xtee:id
xsi:type="xsd:string">6cae248568b3db7e97ff784673a4d38c5906bee0</xtee:id>
<xtee:nimi xsi:type="xsd:string">dhl.sendDocuments.v1</xtee:nimi>
<xtee:toimik xsi:type="xsd:string"></xtee:toimik>
<xtee:amet xsi:type="xsd:string"></xtee:amet>
</SOAP-ENV:Header>
<SOAP-ENV:Body>
  <dhl:sendDocumentsResponse>
    <paring>
      <dokumendid xsi:type="xsd:string">52ed0ffbf27fc34759dce05d0e7bed2302876cec</dokumendid>
      <kaust/>
      <sailitustahtaeg>2009-01-25T23:59:59</sailitustahtaeg>
      <edastus_id>123456789_1201</edastus_id>
      <fragment_nr>-1</fragment_nr>
      <fragmente_kokku>0</fragmente_kokku>
    </paring>
    <keha href="cid:793340a8216da081f3d785bcc74c0f74"/>
  </dhl:sendDocumentsResponse>
</SOAP-ENV:Body>
</SOAP-ENV:Envelope>

--=_9d665408f43f4698f71029c2df2b834e
Content-Type:{http://www.w3.org/2001/XMLSchema}base64Binary
Content-Transfer-Encoding:base64
Content-Encoding: gzip
Content-ID:<793340a8216da081f3d785bcc74c0f74>

VMO1ZWxpbmUgdGFsdiB2w7VpYiBzYWFidWRhIGhpbGplbSBrdWkgdGF2YWxpc2Vs
dC4K

--=_9d665408f43f4698f71029c2df2b834e
```

##### Päringu „dokumendid“ elemendi sisu

```
<dokument xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl/2010/2">
    <metainfo/>
    <transport>
        <saatja>
            <regnr>00000000</regnr>
            <nimi>String</nimi>
            <osakonna_kood>String</osakonna_kood>
            <osakonna_nimi>String</osakonna_nimi>
        </saatja>
        <saaja>
            <regnr>00000000</regnr>
            <nimi>String</nimi>
            <osakonna_kood>String</osakonna_kood>
            <osakonna_nimi>String</osakonna_nimi>
        </saaja>
        <saaja>
            <regnr>00000000</regnr>
            <nimi>String</nimi>
            <osakonna_kood>String</osakonna_kood>
            <osakonna_nimi>String</osakonna_nimi>
        </saaja>
    </transport>
    <ajalugu/>
    <metaxml>
        <p>Siin sees võib olla suvaline XML jada</p>
        <item>Mingeid piiraguid elementidele pole</item>
    </metaxml>
    <failid>
        <kokku>2</kokku>
        <fail>
            <jrknr>1</jrknr>
            <fail_pealkiri>DVK skeem</fail_pealkiri>
            <fail_suurus>3075</fail_suurus>
            <fail_tyyp>text/xml</fail_tyyp>
            <fail_nimi>dhl.xsd</fail_nimi>
            <zip_base64_sisu>
    PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iaXNvLTg4NTktMSI/Pg0KPCEt </zip_base64_sisu>
<pohi_dokument>true</pohi_dokument>
        </fail>
        <fail>
            <jrknr>2</jrknr>
            <fail_pealkiri>DVK skeem</fail_pealkiri>
            <fail_suurus>2679</fail_suurus>
            <fail_tyyp>text/xml</fail_tyyp>
            <fail_nimi>dhl-aadress.xsd</fail_nimi>
            <zip_base64_sisu>
    PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iaXNvLTg4NTktMSI/Pg0KPCEt </zip_base64_sisu>
        </fail>
    </failid>
</dokument>
```

##### Päringu vastuses „keha“ elemendi sisu

Elemendi „keha“ sisu kodeerimata kujul on:
```
<dhl_id>54365435</dhl_id>
```
Kui saadeti korraga mitu dokumenti, siis on elemendi „keha“ sisu kodeerimata kujul selline:
```
<dhl_id>54365435</dhl_id>
<dhl_id>54365436</dhl_id>
<dhl_id>54365437</dhl_id>
```


###sendDocuments.v4
Päring erineb versioonist 3 selle poolest, et teenus võtab vastu kapsli versiooni 2.1.

####Näide:


####Päring
```
POST dhl/services/dhlHttpSoapPort HTTP/1.1
Accept-Encoding: gzip,deflate
Content-Type: multipart/related; type="text/xml"; start="<rootpart@soapui.org>"; boundary="----=_Part_0_316047069.1405334028041"
SOAPAction: ""
MIME-Version: 1.0
Content-Length: 19154

<soapenv:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xmlns:xsd="http://www.w3.org/2001/XMLSchema" 
xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" 
xmlns:dhl="http://localhost:8080/services/dhlHttpSoapPort"
xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd">
   <soapenv:Header>
  <xtee:asutus xsi:type="xsd:string">87654321</xtee:asutus>
  <xtee:andmekogu xsi:type="xsd:string">dhl</xtee:andmekogu>
  <xtee:isikukood xsi:type="xsd:string">EE12345678901</xtee:isikukood>
  <xtee:id xsi:type="xsd:string">6cae248568b3db7e97ff784673a4d38c5906bee0</xtee:id>
  <xtee:nimi xsi:type="xsd:string">dhl.sendDocuments.v4</xtee:nimi>
  <xtee:toimik xsi:type="xsd:string"></xtee:toimik>
  <xtee:amet xsi:type="xsd:string"></xtee:amet>
</soapenv:Header>
<soapenv:Body>
  <dhl:sendDocuments>
    <keha>
      <dokumendid href="cid:uus_kapsel_1.xml.gz.b64"/>
      <kaust/>
    </keha>
  </dhl:sendDocuments>
</soapenv:Body>
</soapenv:Envelope>
```

####Päringu keha sisu, mis on base64 dekodeeritud ning seejärel Gzip'ist lahti pakitud:

```xml
<?xml version="1.0" encoding="utf-8"?>
<DecContainer xmlns="http://www.riik.ee/schemas/deccontainer/vers_2_1/">
  <Transport>
    <DecSender>
      <OrganisationCode>87654321</OrganisationCode>
      <PersonalIdCode>EE38806190294</PersonalIdCode>
    </DecSender>
    <DecRecipient>
      <OrganisationCode>87654321</OrganisationCode>
    </DecRecipient>
  </Transport>
  <Initiator>
    <InitiatorRecordOriginalIdentifier>213465</InitiatorRecordOriginalIdentifier>
    <InitiatorRecordDate>2012-11-11T19:18:03</InitiatorRecordDate>
    <Person>
      <Name>Lauri Tammemäe</Name>
      <GivenName>Lauri</GivenName>
      <Surname>Tammemäe</Surname>
      <PersonalIdCode>EE38806190294</PersonalIdCode>
      <Residency>EE</Residency>
    </Person>
    <ContactData>
      <Adit>1</Adit>
      <Phone>+3726630276</Phone>
      <Email>lauri.tammemae@ria.ee</Email>
      <WebPage>www.hot.ee/lauri</WebPage>
      <MessagingAddress>skype: lauri.tammemae</MessagingAddress>
      <PostalAddress>
        <Country>Eesti</Country>
        <County>Harju maakond</County>
        <LocalGovernment>Tallinna linn</LocalGovernment>
        <AdministrativeUnit>Mustamäe linnaosa</AdministrativeUnit>
        <SmallPlace>Pääsukese KÜ</SmallPlace>
        <LandUnit></LandUnit>
        <Street>Mustamäe tee</Street>
        <HouseNumber>248</HouseNumber>
        <BuildingPartNumber>62</BuildingPartNumber>
        <PostalCode>11212</PostalCode>
      </PostalAddress>
    </ContactData>
  </Initiator>
  <RecordCreator>
    <Organisation>
      <Name>Majandus- ja Kommunikatsiooniministeerium</Name>
      <OrganisationCode>EE70003158</OrganisationCode>
      <StructuralUnit>Infoühiskonna teenuste arendamise osakond</StructuralUnit>
      <PositionTitle>nõunik</PositionTitle>
      <Residency>EE</Residency>
    </Organisation>
    <Person>
      <Name>Liivi Karpištšenko</Name>
      <GivenName>Liivi</GivenName>
      <Surname>Karpištšenko</Surname>
      <PersonalIdCode></PersonalIdCode>
      <Residency>EE</Residency>
    </Person>
    <ContactData>
      <Adit>0</Adit>
      <Phone>6256342</Phone>
      <Email>info@mkm.ee</Email>
      <WebPage>www.mkm.ee</WebPage>
      <PostalAddress>
        <Country>Eesti</Country>
        <County>Harju maakond</County>
        <LocalGovernment>Tallinna linn</LocalGovernment>
        <AdministrativeUnit>Kesklinna linnaosa</AdministrativeUnit>
        <SmallPlace></SmallPlace>
        <LandUnit></LandUnit>
        <Street>Harju tänav</Street>
        <HouseNumber>11</HouseNumber>
        <BuildingPartNumber>126</BuildingPartNumber>
        <PostalCode>15072</PostalCode>
      </PostalAddress>
    </ContactData>
  </RecordCreator>
  <RecordSenderToDec>
    <Person>
      <Name>Liivi Karpištšenko</Name>
      <GivenName>Liivi</GivenName>
      <Surname>Karpištšenko</Surname>
      <PersonalIdCode></PersonalIdCode>
      <Residency>EE</Residency>
    </Person>
    <ContactData>
      <Adit>0</Adit>
      <Phone>6256342</Phone>
      <Email>info@mkm.ee</Email>
      <WebPage>www.mkm.ee</WebPage>
      <PostalAddress>
        <Country>Eesti</Country>
        <County>Harju maakond</County>
        <LocalGovernment>Tallinna linn</LocalGovernment>
        <AdministrativeUnit>Kesklinna linnaosa</AdministrativeUnit>
        <SmallPlace></SmallPlace>
        <LandUnit></LandUnit>
        <Street>Harju tänav</Street>
        <HouseNumber>11</HouseNumber>
        <BuildingPartNumber>126</BuildingPartNumber>
        <PostalCode>15072</PostalCode>
      </PostalAddress>
    </ContactData>
  </RecordSenderToDec>
  <Recipient>
    <RecipientRecordGuid>25892e17-80f6-415f-9c65-7395632f0223</RecipientRecordGuid>
    <RecipientRecordOriginalIdentifier>1.3P-41</RecipientRecordOriginalIdentifier>
    <MessageForRecipient>Teadmiseks</MessageForRecipient>
    <Organisation>
      <Name>Riigi Infosüsteemi Amet</Name>
      <OrganisationCode>EE70006317</OrganisationCode>
      <Residency>EE</Residency>
    </Organisation>
    <Person>
      <Name>Kadi Krull</Name>
      <GivenName>Kadi</GivenName>
      <Surname>Krull</Surname>
      <Residency>EE</Residency>
    </Person>
  </Recipient>
  <Recipient>
    <RecipientRecordOriginalIdentifier>213465</RecipientRecordOriginalIdentifier>
    <Person>
      <Name>Lauri Tammemäe</Name>
      <GivenName>Lauri</GivenName>
      <Surname>Tammemäe</Surname>
      <PersonalIdCode>EE38806190294</PersonalIdCode>
      <Residency>EE</Residency>
    </Person>
    <ContactData>
      <Adit>1</Adit>
      <Phone>+3726630276</Phone>
      <Email>lauri.tammemae@ria.ee</Email>
      <WebPage>www.hot.ee/lauri</WebPage>
      <MessagingAddress>skype: lauri.tammemae</MessagingAddress>
      <PostalAddress>
        <Country>Eesti</Country>
        <County>Harju maakond</County>
        <LocalGovernment>Tallinna linn</LocalGovernment>
        <AdministrativeUnit>Mustamäe linnaosa</AdministrativeUnit>
        <SmallPlace>Pääsukese KÜ</SmallPlace>
        <LandUnit></LandUnit>
        <Street>Mustamäe tee</Street>
        <HouseNumber>248</HouseNumber>
        <BuildingPartNumber>62</BuildingPartNumber>
        <PostalCode>11212</PostalCode>
      </PostalAddress>
    </ContactData>
  </Recipient>
  <RecordMetadata>
    <RecordGuid>25892e17-80f6-415f-9c65-7395632f0211</RecordGuid>
    <RecordType>Kiri</RecordType>
    <RecordOriginalIdentifier>12.1/125</RecordOriginalIdentifier>
    <RecordDateRegistered>2012-11-20T09:45:55</RecordDateRegistered>
    <RecordTitle>Vastus kodaniku ettepanekule</RecordTitle>
    <RecordLanguage>EE</RecordLanguage>
    <RecordAbstract>Kodaniku ettepanek registreeritud...</RecordAbstract>
  </RecordMetadata>
  <Access>
    <AccessConditionsCode>Avalik</AccessConditionsCode>
  </Access>
  <SignatureMetadata>
    <SignatureType>Digitaalallkiri</SignatureType>
    <Signer>Juhan Parts</Signer>
    <Verified>allkiri on kehtiv</Verified>
    <SignatureVerificationDate>2012-11-20T12:12:12</SignatureVerificationDate>
  </SignatureMetadata>
  <File>
    <FileGuid>30891e17-66f4-468f-9c79-8315632f0314</FileGuid>
    <RecordMainComponent>1</RecordMainComponent>
    <FileName>Vastus ettepanekule.ddoc</FileName>
    <FileSize>415998</FileSize>
    <ZipBase64Content>...</ZipBase64Content>
  </File>
  <RecordTypeSpecificMetadata />
</DecContainer>
```

####Päringu vastus:

```
HTTP/1.1 200 OK
Server: Apache-Coyote/1.1
Content-Type: multipart/related; type="text/xml"; start="<5574F4BD89CBE8B76D6026DA6266F8AC>";   boundary="----=_Part_0_49658970.1405334024943"
Transfer-Encoding: chunked
Date: Mon, 14 Jul 2014 10:33:45 GMT


------=_Part_0_49658970.1405334024943
Content-Type: text/xml; charset=UTF-8
Content-Transfer-Encoding: binary
Content-Id: <5574F4BD89CBE8B76D6026DA6266F8AC>

<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
                                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                                 xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd">
    <soapenv:Header>
        <xtee:asutus xsi:type="xsd:string">87654321</xtee:asutus>
        <xtee:andmekogu xsi:type="xsd:string">dhl</xtee:andmekogu>
        <xtee:isikukood xsi:type="xsd:string">EE12345678901</xtee:isikukood>
        <xtee:id xsi:type="xsd:string">6cae248568b3db7e97ff784673a4d38c5906bee0</xtee:id>
        <xtee:nimi xsi:type="xsd:string">dhl.sendDocuments.v4</xtee:nimi>
        <xtee:toimik xsi:type="xsd:string"/>
        <xtee:amet xsi:type="xsd:string"/>
    </soapenv:Header>
    <soapenv:Body>
        <sendDocumentsResponse xmlns="">
            <paring xmlns="">
                <dokumendid>F295F2F85CA72412D08F04165B813700</dokumendid>
                <kaust></kaust>
            </paring>
            <keha href="cid:14ABD2D6837F5F3FC8C201A39BB4AA62" xmlns=""/>
        </sendDocumentsResponse>
    </soapenv:Body>
</soapenv:Envelope>

------=_Part_0_49658970.1405334024943
Content-Type: {http://www.w3.org/2001/XMLSchema}base64Binary
Content-Transfer-Encoding: binary
Content-Id: <14ABD2D6837F5F3FC8C201A39BB4AA62>
Content-Encoding: gzip

H4sIAAAAAAAAALPJTs1ItLNJyciJz0yxMzMxMrbRh3Js9MFyAK8AUnUiAAAA
------=_Part_0_49658970.1405334024943--
```

Vastuse keha sisu pärast base64 dekodeerimise ning Gzip'ist lahti
pakkimist:
```
<keha><dhl_id>6423</dhl_id></keha>
```

###getSendStatus
Päringu nimi: dhl.getSendStatus.v1

Sisendi keha: base64Binary

Väljundi keha: base64Binary

Sisendi kehaks on base64 kodeeringus documentRefsArrayType tüüpi
massiiv, mis sisaldab DVK poolt dokumentidele omistatud unikaalseid
ID-e, mille kohta saatmisinfot soovitakse saada.

Väljundi kehaks on base64 kodeeringus massiiv, mille elemendid „item“ on
struktuurid elementidega:

-   dhl\_id – dokumendile DVK poolt omistatud unikaalne id
-   edastus – null kuni mitu elementi, millest igaüks kirjeldab
    konkreetse edastuse infot (vt punkt „Edastatud dokumentide
    staatuse kontroll“)
-   olek – dokumendi edastamise koondolek

Olekute täpsema kirjelduse leiad käesoleva dokumendi peatüki
„Dokumentide logistika” alampeatükist „[Edastatud dokumentide staatuse
kontroll](#anchor-245)”.

#### <span id="anchor-367"></span><span id="anchor-368"></span><span id="anchor-367"></span>Näide

##### Päring

POST /cgi-bin/consumer\_proxy HTTP/1.0

Content-Type: multipart/related; type=text/xml;

boundary="=\_b5a8d09eeeb161be29def84633d6f6fc"

SOAPAction: ""

--=\_b5a8d09eeeb161be29def84633d6f6fc

Content-Type:text/xml; charset="UTF-8"

Content-Transfer-Encoding:8bit

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;SOAP-ENV:Envelope

xmlns:SOAPENV="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;SOAP-ENV:Header&gt;

&lt;xtee:asutus xsi:type="xsd:string"&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu xsi:type="xsd:string"&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:isikukood
xsi:type="xsd:string"&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;xtee:id
xsi:type="xsd:string"&gt;6cae248568b3db7e97ff784673a4d38c5906bee0&lt;/xtee:id&gt;

&lt;xtee:nimi
xsi:type="xsd:string"&gt;dhl.getSendStatus.v1&lt;/xtee:nimi&gt;

&lt;xtee:toimik xsi:type="xsd:string"&gt;&lt;/xtee:toimik&gt;

&lt;xtee:amet xsi:type="xsd:string"&gt;&lt;/xtee:amet&gt;

&lt;/SOAP-ENV:Header&gt;

&lt;SOAP-ENV:Body&gt;

&lt;dhl:getSendStatus&gt;

&lt;keha href="cid:b8fdc418df27ba3095a2d21b7be6d802"/&gt;

&lt;/dhl:getSendStatus&gt;

&lt;/SOAP-ENV:Body&gt;

&lt;/SOAP-ENV:Envelope&gt;

--=\_b5a8d09eeeb161be29def84633d6f6fc

Content-Disposition:php5hmCiX

Content-Type:{http://www.w3.org/2001/XMLSchema}base64Binary

Content-Transfer-Encoding:base64

Content-Encoding: gzip

Content-ID:&lt;b8fdc418df27ba3095a2d21b7be6d802&gt;

a29ybmkJa3cNCmtvcnNpa2EJY28NCmtyZWVrYQllbA0Ka3JpaQn1ZA0Ka3Vt9Wtp

…

dg0KbmVwYWxpCW5lDQpuaXZoaQnkdA0KbmphbmT+YQlueQ0Kbm9nYWkJ9ncNCg==

--=\_b5a8d09eeeb161be29def84633d6f6fc

##### Päringu vastus

HTTP/1.1 200 OK

Content-Type: multipart/related; type=text/xml;

boundary="=\_9d665408f43f4698f71029c2df2b834e"

--=\_9d665408f43f4698f71029c2df2b834e

Content-Type:text/xml; charset="UTF-8"

Content-Transfer-Encoding:8bit

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;SOAP-ENV:Envelope xmlns:SOAPENV="

http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;SOAP-ENV:Header&gt;

&lt;xtee:asutus xsi:type="xsd:string"&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu xsi:type="xsd:string"&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:isikukood
xsi:type="xsd:string"&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;xtee:id

xsi:type="xsd:string"&gt;6cae248568b3db7e97ff784673a4d38c5906bee0&lt;/xtee:id&gt;

&lt;xtee:nimi
xsi:type="xsd:string"&gt;dhl.getSendStatus.v1&lt;/xtee:nimi&gt;

&lt;xtee:toimik xsi:type="xsd:string"&gt;&lt;/xtee:toimik&gt;

&lt;xtee:amet xsi:type="xsd:string"&gt;&lt;/xtee:amet&gt;

&lt;/SOAP-ENV:Header&gt;

&lt;SOAP-ENV:Body&gt;

&lt;dhl:getSendStatusResponse&gt;

&lt;paring
xsi:type="xsd:string"&gt;52ed0ffbf27fc34759dce05d0e7bed2302876cec&lt;/paring&gt;

&lt;keha href="cid:793340a8216da081f3d785bcc74c0f74"/&gt;

&lt;/dhl:getSendStatusResponse&gt;

&lt;/SOAP-ENV:Body&gt;

&lt;/SOAP-ENV:Envelope&gt;

--=\_9d665408f43f4698f71029c2df2b834e

Content-Type:{http://www.w3.org/2001/XMLSchema}base64Binary

Content-Transfer-Encoding:base64

Content-Encoding: gzip

Content-ID:&lt;793340a8216da081f3d785bcc74c0f74&gt;

VMO1ZWxpbmUgdGFsdiB2w7VpYiBzYWFidWRhIGhpbGplbSBrdWkgdGF2YWxpc2Vs

dC4K

--=\_9d665408f43f4698f71029c2df2b834e

##### Päringu „keha“ elemendi sisu

Elemendi „keha“ sisu kodeerimata kujul on:

*&lt;dhl\_id&gt;54365435&lt;/dhl\_id&gt;*

##### Päringu vastuse „keha“ elemendi sisu

Elemendi „keha“ sisu kodeerimata kujul on:

*&lt;item&gt;*

*&lt;dhl\_id&gt;54365435&lt;/dhl\_id&gt;*

*&lt;edastus&gt;*

*&lt;saaja&gt;*

*&lt;regnr&gt;12345678&lt;/regnr&gt;*

*&lt;nimi&gt;Saaja asutuse nimi&lt;/nimi&gt;*

*&lt;osakona\_kood&gt;&lt;/osakona\_kood&gt;*

*&lt;osakonna\_nimi&gt;&lt;/osakonna\_nimi&gt;*

*&lt;isikukood&gt;30101010001&lt;/isikukood&gt;*

*&lt;epost&gt;isik1@asutus1.ee&lt;/epost&gt;*

*&lt;/saaja&gt;*

*&lt;saadud&gt;2005-09-01T12:34:02&lt;/saadud&gt;*

*&lt;meetod&gt;xtee&lt;/meetod&gt;*

*&lt;edastatud&gt;2005-09-01T12:34:21&lt;/edastatud&gt;*

*&lt;loetud&gt;2005-09-01T12:35:11&lt;/loetud&gt;*

*&lt;staatus&gt;saadetud&lt;/staatus&gt;*

*&lt;vastuvotja\_staatus\_id&gt;10&lt;/vastuvotja\_staatus\_id&gt;*

*&lt;fault&gt;*

*&lt;faultcode&gt;100&lt;/faultcode&gt;*

*&lt;faultactor&gt;CLIENT&lt;/faultactor&gt;*

*&lt;faultstring&gt;Veateade&lt;/faultstring&gt;*

*&lt;faultdetail&gt;Vea kirjeldus&lt;/faultdetail&gt;*

*&lt;/fault&gt;*

*&lt;metaxml&gt;*

*&lt;andmevali1&gt;1. andmevälja väärtus&lt;/andmevali1&gt;*

*&lt;andmevali2&gt;2. andmevälja väärtus&lt;/andmevali2&gt;*

*&lt;/metaxml&gt;*

*&lt;/edastus&gt;*

*&lt;edastus&gt;*

*&lt;saaja&gt;*

*&lt;regnr&gt;87654321&lt;/regnr&gt;*

*&lt;nimi&gt;Teise saaja nimi&lt;/nimi&gt;*

*&lt;osakona\_kood&gt;&lt;/osakona\_kood&gt;*

*&lt;osakonna\_nimi&gt;&lt;/osakonna\_nimi&gt;*

*&lt;isikukood&gt;40101010001&lt;/isikukood&gt;*

*&lt;epost&gt;isik2@asutus2.ee&lt;/epost&gt;*

*&lt;/saaja&gt;*

*&lt;saadud&gt;2005-09-01T12:34:02&lt;/saadud&gt;*

*&lt;meetod&gt;xtee&lt;/meetod&gt;*

*&lt;staatus&gt;saatmisel&lt;/staatus&gt;*

*&lt;metaxml&gt;Teksti kujul metaandmed&lt;/metaxml&gt;*

*&lt;/edastus&gt;*

*&lt;olek&gt;saatmisel&lt;/olek&gt;*

*&lt;/item&gt;*


Päringu nimi: dhl.getSendStatus.v2

Sisendi keha: Struct

dokumendidbase64Binary

staatuse\_ajaluguboolean

Väljundi keha: base64Binary

„dokumendid“ element sisendi kehas on base64 kodeeringus
documentRefsArrayType tüüpi massiiv, mis sisaldab DVK poolt
dokumentidele omistatud unikaalseid ID-e, mille kohta saatmisinfot
soovitakse saada. „staatuse\_ajalugu“ parameetriga (true või false) saab
määrata, kas päringu vastus sisaldab ka dokumentide staatuse ajalugu.
Alternatiiv on kasutada dokumendi identifitseerimiseks dokumendi GUID-i.
Selleks kasutatakse päringu sisendiks massiivi kujul:

*&lt;item&gt;*

*&lt;dokument\_guid&gt;25892e17-80f6-415f-9c65-7395632f0223&lt;/dokument\_guid&gt;*

*&lt;/item&gt;*

Väljundi kehaks on base64 kodeeringus massiiv, mille elemendid „item“ on
struktuurid elementidega:

-   dhl\_id – dokumendile DVK poolt omistatud unikaalne id
-   edastus – null kuni mitu elementi, millest igaüks kirjeldab
    konkreetse edastuse infot (vt punkt „Edastatud dokumentide
    staatuse kontroll“)
-   staatuse\_ajalugu – massiiv dokumendi kõigi seniste staatuse
    muudatuste andmetega adressaatide lõikes.
-   olek – dokumendi edastamise koondolek

Olekute täpsema kirjelduse leiad käesoleva dokumendi peatüki
„Dokumentide logistika” alampeatükist „[Edastatud dokumentide staatuse
kontroll](#anchor-245)”.

#### Näide

##### Päring

POST /cgi-bin/consumer\_proxy HTTP/1.0

Content-Type: multipart/related; type=text/xml;

boundary="=\_b5a8d09eeeb161be29def84633d6f6fc"

SOAPAction: ""

--=\_b5a8d09eeeb161be29def84633d6f6fc

Content-Type:text/xml; charset="UTF-8"

Content-Transfer-Encoding:8bit

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;SOAP-ENV:Envelope

xmlns:SOAPENV="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;SOAP-ENV:Header&gt;

&lt;xtee:asutus xsi:type="xsd:string"&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu xsi:type="xsd:string"&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:isikukood
xsi:type="xsd:string"&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;xtee:id
xsi:type="xsd:string"&gt;6cae248568b3db7e97ff784673a4d38c5906bee0&lt;/xtee:id&gt;

&lt;xtee:nimi
xsi:type="xsd:string"&gt;dhl.getSendStatus.v2&lt;/xtee:nimi&gt;

&lt;xtee:toimik xsi:type="xsd:string"&gt;&lt;/xtee:toimik&gt;

&lt;xtee:amet xsi:type="xsd:string"&gt;&lt;/xtee:amet&gt;

&lt;/SOAP-ENV:Header&gt;

&lt;SOAP-ENV:Body&gt;

&lt;dhl:getSendStatus&gt;

&lt;keha&gt;

&lt;dokumendid href="cid:b8fdc418df27ba3095a2d21b7be6d802"/&gt;

&lt;staatuse\_ajalugu&gt;true&lt;/staatuse\_ajalugu&gt;

&lt;/dhl:getSendStatus&gt;

&lt;/SOAP-ENV:Body&gt;

&lt;/SOAP-ENV:Envelope&gt;

--=\_b5a8d09eeeb161be29def84633d6f6fc

Content-Disposition:php5hmCiX

Content-Type:{http://www.w3.org/2001/XMLSchema}base64Binary

Content-Transfer-Encoding:base64

Content-Encoding: gzip

Content-ID:&lt;b8fdc418df27ba3095a2d21b7be6d802&gt;

a29ybmkJa3cNCmtvcnNpa2EJY28NCmtyZWVrYQllbA0Ka3JpaQn1ZA0Ka3Vt9Wtp

…

dg0KbmVwYWxpCW5lDQpuaXZoaQnkdA0KbmphbmT+YQlueQ0Kbm9nYWkJ9ncNCg==

--=\_b5a8d09eeeb161be29def84633d6f6fc

##### Päringu vastus

HTTP/1.1 200 OK

Content-Type: multipart/related; type=text/xml;

boundary="=\_9d665408f43f4698f71029c2df2b834e"

--=\_9d665408f43f4698f71029c2df2b834e

Content-Type:text/xml; charset="UTF-8"

Content-Transfer-Encoding:8bit

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;SOAP-ENV:Envelope xmlns:SOAPENV="

http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;SOAP-ENV:Header&gt;

&lt;xtee:asutus xsi:type="xsd:string"&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu xsi:type="xsd:string"&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:isikukood
xsi:type="xsd:string"&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;xtee:id

xsi:type="xsd:string"&gt;6cae248568b3db7e97ff784673a4d38c5906bee0&lt;/xtee:id&gt;

&lt;xtee:nimi
xsi:type="xsd:string"&gt;dhl.getSendStatus.v1&lt;/xtee:nimi&gt;

&lt;xtee:toimik xsi:type="xsd:string"&gt;&lt;/xtee:toimik&gt;

&lt;xtee:amet xsi:type="xsd:string"&gt;&lt;/xtee:amet&gt;

&lt;/SOAP-ENV:Header&gt;

&lt;SOAP-ENV:Body&gt;

&lt;dhl:getSendStatusResponse&gt;

&lt;paring&gt;

&lt;dokumendid&gt;52ed0ffbf27fc34759dce05d0e7bed2302876cec&lt;/dokumendid&gt;

&lt;staatuse\_ajalugu&gt;true&lt;/staatuse\_ajalugu&gt;

&lt;/paring&gt;

&lt;keha href="cid:793340a8216da081f3d785bcc74c0f74"/&gt;

&lt;/dhl:getSendStatusResponse&gt;

&lt;/SOAP-ENV:Body&gt;

&lt;/SOAP-ENV:Envelope&gt;

--=\_9d665408f43f4698f71029c2df2b834e

Content-Type:{http://www.w3.org/2001/XMLSchema}base64Binary

Content-Transfer-Encoding:base64

Content-Encoding: gzip

Content-ID:&lt;793340a8216da081f3d785bcc74c0f74&gt;

VMO1ZWxpbmUgdGFsdiB2w7VpYiBzYWFidWRhIGhpbGplbSBrdWkgdGF2YWxpc2Vs

dC4K

--=\_9d665408f43f4698f71029c2df2b834e

##### Päringu „dokumendid“ elemendi sisu

Elemendi „keha“ sisu kodeerimata kujul on (kasutatakse dokumendi DVK
unikaalset ID-d):

*&lt;item&gt;*

*&lt;dhl\_id&gt;54365435&lt;/dhl\_id&gt;*

*&lt;/item&gt;*

Elemendi „keha“ sisu kodeerimata kujul on (kasutatakse dokumendi
GUID-i):

*&lt;item&gt;*

*&lt;dokument\_guid&gt;25892e17-80f6-415f-9c65-7395632f0223&lt;/dokument\_guid&gt;*

*&lt;/item&gt;*

##### Päringu vastuse „keha“ elemendi sisu

Elemendi „keha“ sisu kodeerimata kujul on:

*&lt;item&gt;*

*&lt;dhl\_id&gt;54365435&lt;/dhl\_id&gt;\
&lt;dokument\_guid&gt;25892e17-80f6-415f-9c65-7395632f0223&lt;/dokument\_guid&gt;\
&lt;edastus&gt;*

*&lt;saaja&gt;*

*&lt;regnr&gt;12345678&lt;/regnr&gt;*

*&lt;nimi&gt;Saaja asutuse nimi&lt;/nimi&gt;*

*&lt;osakona\_kood&gt;&lt;/osakona\_kood&gt;*

*&lt;osakonna\_nimi&gt;&lt;/osakonna\_nimi&gt;*

*&lt;isikukood&gt;30101010001&lt;/isikukood&gt;*

*&lt;epost&gt;isik1@asutus1.ee&lt;/epost&gt;*

*&lt;/saaja&gt;*

*&lt;saadud&gt;2005-09-01T12:34:02&lt;/saadud&gt;*

*&lt;meetod&gt;xtee&lt;/meetod&gt;*

*&lt;edastatud&gt;2005-09-01T12:34:21&lt;/edastatud&gt;*

*&lt;loetud&gt;2005-09-01T12:35:11&lt;/loetud&gt;*

*&lt;staatus&gt;saadetud&lt;/staatus&gt;*

*&lt;vastuvotja\_staatus\_id&gt;10&lt;/vastuvotja\_staatus\_id&gt;*

*&lt;fault&gt;*

*&lt;faultcode&gt;100&lt;/faultcode&gt;*

*&lt;faultactor&gt;CLIENT&lt;/faultactor&gt;*

*&lt;faultstring&gt;Veateade&lt;/faultstring&gt;*

*&lt;faultdetail&gt;Vea kirjeldus&lt;/faultdetail&gt;*

*&lt;/fault&gt;*

*&lt;metaxml&gt;*

*&lt;andmevali1&gt;1. andmevälja väärtus&lt;/andmevali1&gt;*

*&lt;andmevali2&gt;2. andmevälja väärtus&lt;/andmevali2&gt;*

*&lt;/metaxml&gt;*

*&lt;/edastus&gt;*

*&lt;edastus&gt;*

*&lt;saaja&gt;*

*&lt;regnr&gt;87654321&lt;/regnr&gt;*

*&lt;nimi&gt;Teise saaja nimi&lt;/nimi&gt;*

*&lt;osakona\_kood&gt;&lt;/osakona\_kood&gt;*

*&lt;osakonna\_nimi&gt;&lt;/osakonna\_nimi&gt;*

*&lt;isikukood&gt;40101010001&lt;/isikukood&gt;*

*&lt;epost&gt;isik2@asutus2.ee&lt;/epost&gt;*

*&lt;/saaja&gt;*

*&lt;saadud&gt;2005-09-01T12:34:02&lt;/saadud&gt;*

*&lt;meetod&gt;xtee&lt;/meetod&gt;*

*&lt;staatus&gt;saatmisel&lt;/staatus&gt;*

*&lt;metaxml&gt;Teksti kujul metaandmed&lt;/metaxml&gt;*

*&lt;/edastus&gt;*

*&lt;staatuse\_ajalugu&gt;*

*&lt;staatus&gt;*

*&lt;saaja&gt;*

*&lt;regnr&gt;12345678&lt;/regnr&gt;*

*&lt;/saaja&gt;*

*&lt;staatuse\_ajalugu\_id&gt;28&lt;/staatuse\_ajalugu\_id&gt;*

*&lt;staatuse\_muutmise\_aeg&gt;2005-09-01T12:34:02&lt;/staatuse\_muutmise\_aeg&gt;*

*&lt;staatus&gt;saatmisel&lt;/staatus&gt;*

*&lt;vastuvotja\_staatus\_id&gt;0&lt;/vastuvotja\_staatus\_id&gt;*

*&lt;/staatus&gt;*

*&lt;staatus&gt;*

*&lt;saaja&gt;*

*&lt;regnr&gt;12345678&lt;/regnr&gt;*

*&lt;/saaja&gt;*

*&lt;staatuse\_ajalugu\_id&gt;29&lt;/staatuse\_ajalugu\_id&gt;*

*&lt;staatuse\_muutmise\_aeg&gt;2005-09-01T12:34:21&lt;/staatuse\_muutmise\_aeg&gt;*

*&lt;staatus&gt;saadetud&lt;/staatus&gt;*

*&lt;vastuvotja\_staatus\_id&gt;0&lt;/vastuvotja\_staatus\_id&gt;*

*&lt;/staatus&gt;*

*&lt;staatus&gt;*

*&lt;saaja&gt;*

*&lt;regnr&gt;87654321&lt;/regnr&gt;*

*&lt;/saaja&gt;*

*&lt;staatuse\_ajalugu\_id&gt;26&lt;/staatuse\_ajalugu\_id&gt;*

*&lt;staatuse\_muutmise\_aeg&gt;2005-09-01T12:34:02&lt;/staatuse\_muutmise\_aeg&gt;*

*&lt;staatus&gt;saatmisel&lt;/staatus&gt;*

*&lt;vastuvotja\_staatus\_id&gt;0&lt;/vastuvotja\_staatus\_id&gt;*

*&lt;/staatus&gt;*

*&lt;/staatuse\_ajalugu&gt;*

*&lt;olek&gt;saatmisel&lt;/olek&gt;*


Kui vastuvõtjale on saadetud 2.1 versioon kapslist ja asutuse toetatav
kapsli versioon on 1.0, siis kapsel konverteeritakse kapsli versioonist
2.1 versiooni 1.0. NB! Teistpidi konverteerimist ei eksisteeri.


Päringu nimi: dhl.receiveDocuments.v1

Sisendi keha: Struct

integer arv

string\[\] kaust

Väljundi keha: base64Binary

Element „arv“ määrab ära maksimaalse loetavate dokumentide arvu. Element
võib puududa, sellisel juhul tagastatakse vaikimisi 10 dokumenti.

Element „kaust“ määrab ära, millisest DVK kaustast dokumendid loetakse.
Element võib ka puududa (või olla väärtustamata), sellisel juhul
tagastatakse vaikimisi dokumendid kõigist kaustadest. Kui elemendiga
„kaust” on ette antud kaust, mida DVK-s ei eksisteeri, siis ei tagastata
ühtegi dokumenti.

Väljund on base64 kodeeringus documentsArrayType tüüpi massiiv, mille
iga element on tagastatud dokument.

#### <span id="anchor-410"></span>Näide

##### Päring

POST /cgi-bin/consumer\_proxy HTTP/1.0

Content-Type: text/xml

SOAPAction: ""

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;SOAP-ENV:Envelope

xmlns:SOAPENV="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;SOAP-ENV:Header&gt;

&lt;xtee:asutus xsi:type="xsd:string"&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu xsi:type="xsd:string"&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:isikukood
xsi:type="xsd:string"&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;xtee:id
xsi:type="xsd:string"&gt;6cae248568b3db7e97ff784673a4d38c5906bee0&lt;/xtee:id&gt;

&lt;xtee:nimi
xsi:type="xsd:string"&gt;dhl.receiveDocuments.v1&lt;/xtee:nimi&gt;

&lt;xtee:toimik xsi:type="xsd:string"&gt;&lt;/xtee:toimik&gt;

&lt;xtee:amet xsi:type="xsd:string"&gt;&lt;/xtee:amet&gt;

&lt;/SOAP-ENV:Header&gt;

&lt;SOAP-ENV:Body&gt;

&lt;dhl:receiveDocuments&gt;

&lt;keha&gt;

&lt;arv xsi:type="xsd:integer"&gt;10&lt;/arv&gt;

&lt;kaust/&gt;

&lt;/keha&gt;

&lt;/dhl:receiveDocuments&gt;

&lt;/SOAP-ENV:Body&gt;

&lt;/SOAP-ENV:Envelope&gt;

##### Päring mitmest kaustast vastuvõtmise korral

POST /cgi-bin/consumer\_proxy HTTP/1.0

Content-Type: text/xml

SOAPAction: ""

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;SOAP-ENV:Envelope

xmlns:SOAPENV="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;SOAP-ENV:Header&gt;

&lt;xtee:asutus xsi:type="xsd:string"&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu xsi:type="xsd:string"&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:isikukood
xsi:type="xsd:string"&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;xtee:id
xsi:type="xsd:string"&gt;6cae248568b3db7e97ff784673a4d38c5906bee0&lt;/xtee:id&gt;

&lt;xtee:nimi
xsi:type="xsd:string"&gt;dhl.receiveDocuments.v1&lt;/xtee:nimi&gt;

&lt;xtee:toimik xsi:type="xsd:string"&gt;&lt;/xtee:toimik&gt;

&lt;xtee:amet xsi:type="xsd:string"&gt;&lt;/xtee:amet&gt;

&lt;/SOAP-ENV:Header&gt;

&lt;SOAP-ENV:Body&gt;

&lt;dhl:receiveDocuments&gt;

&lt;keha&gt;

&lt;arv xsi:type="xsd:integer"&gt;10&lt;/arv&gt;

&lt;kaust&gt;/ARVED&lt;/kaust&gt;

&lt;kaust&gt;/EVORMID&lt;/kaust&gt;

&lt;kaust&gt;/PKD&lt;/kaust&gt;

&lt;/keha&gt;

&lt;/dhl:receiveDocuments&gt;

&lt;/SOAP-ENV:Body&gt;

&lt;/SOAP-ENV:Envelope&gt;

##### Päringu vastus

HTTP/1.1 200 OK

Content-Type: multipart/related; type=text/xml;

boundary="=\_9d665408f43f4698f71029c2df2b834e"

--=\_9d665408f43f4698f71029c2df2b834e

Content-Type:text/xml; charset="UTF-8"

Content-Transfer-Encoding:8bit

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;SOAP-ENV:Envelope xmlns:SOAPENV="

http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;SOAP-ENV:Header&gt;

&lt;xtee:asutus xsi:type="xsd:string"&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu xsi:type="xsd:string"&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:isikukood
xsi:type="xsd:string"&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;xtee:id

xsi:type="xsd:string"&gt;6cae248568b3db7e97ff784673a4d38c5906bee0&lt;/xtee:id&gt;

&lt;xtee:nimi
xsi:type="xsd:string"&gt;dhl.receiveDocuments.v1&lt;/xtee:nimi&gt;

&lt;xtee:toimik xsi:type="xsd:string"&gt;&lt;/xtee:toimik&gt;

&lt;xtee:amet xsi:type="xsd:string"&gt;&lt;/xtee:amet&gt;

&lt;/SOAP-ENV:Header&gt;

&lt;SOAP-ENV:Body&gt;

&lt;dhl:receiveDocumentsResponse&gt;

&lt;paring&gt;

&lt;arv xsi:type="xsd:integer"&gt;10&lt;/arv&gt;

&lt;kaust/&gt;

&lt;/paring&gt;

&lt;keha href="cid:793340a8216da081f3d785bcc74c0f74"/&gt;

&lt;/dhl:receiveDocumentsResponse&gt;

&lt;/SOAP-ENV:Body&gt;

&lt;/SOAP-ENV:Envelope&gt;

--=\_9d665408f43f4698f71029c2df2b834e

Content-Type:{http://www.w3.org/2001/XMLSchema}base64Binary

Content-Transfer-Encoding:base64

Content-Encoding: gzip

Content-ID:&lt;793340a8216da081f3d785bcc74c0f74&gt;

VMO1ZWxpbmUgdGFsdiB2w7VpYiBzYWFidWRhIGhpbGplbSBrdWkgdGF2YWxpc2Vs

dC4K

--=\_9d665408f43f4698f71029c2df2b834e

##### Päringu vastuse „keha“ elemendi sisu

Elemendi „keha“ sisu kodeerimata kujul on:

*&lt;dokument&gt;*

*&lt;metainfo&gt;*

*&lt;dhl\_id&gt;54365435&lt;/dhl\_id&gt;*

*&lt;/metainfo&gt;*

*&lt;transport&gt;*

*&lt;saatja&gt;*

*&lt;regnr&gt;00000000&lt;/regnr&gt;*

*&lt;nimi&gt;String&lt;/nimi&gt;*

*&lt;osakonna\_kood&gt;String&lt;/osakonna\_kood&gt;*

*&lt;osakonna\_nimi&gt;String&lt;/osakonna\_nimi&gt;*

*&lt;/saatja&gt;*

*&lt;saaja&gt;*

*&lt;regnr&gt;00000000&lt;/regnr&gt;*

*&lt;nimi&gt;String&lt;/nimi&gt;*

*&lt;osakonna\_kood&gt;String&lt;/osakonna\_kood&gt;*

*&lt;osakonna\_nimi&gt;String&lt;/osakonna\_nimi&gt;*

*&lt;/saaja&gt;*

*&lt;saaja&gt;*

*&lt;regnr&gt;00000000&lt;/regnr&gt;*

*&lt;nimi&gt;String&lt;/nimi&gt;*

*&lt;osakonna\_kood&gt;String&lt;/osakonna\_kood&gt;*

*&lt;osakonna\_nimi&gt;String&lt;/osakonna\_nimi&gt;*

*&lt;/saaja&gt;*

*&lt;/transport&gt;*

*&lt;ajalugu/&gt;*

*&lt;metaxml&gt;*

*&lt;p&gt;Siin sees võib olla suvaline XML jada&lt;/p&gt;*

*&lt;item&gt;Mingeid piiraguid elementidele pole&lt;/item&gt;*

*&lt;/metaxml&gt;*

*&lt;SignedDoc format="DIGIDOC-XML" version="1.3"
xmlns="http://www.sk.ee/DigiDoc/v1.3.0\#"&gt;*

*&lt;DataFile ContentType="EMBEDDED\_BASE64" Filename="dhl.xsd" Id="D0"
MimeType="text/xml" Size="3075"
xmlns="http://www.sk.ee/DigiDoc/v1.3.0\#"&gt;PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iaXNvLTg4NTktMSI/Pg0KPCEt*

*&lt;/DataFile&gt;*

*&lt;DataFile ContentType="EMBEDDED\_BASE64" Filename="dhl-aadress.xsd"
Id="D1" MimeType="text/xml" Size="2679"
xmlns="http://www.sk.ee/DigiDoc/v1.3.0\#"&gt;PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iaXNvLTg4NTktMSI/Pg0KPCEt*

*&lt;/DataFile&gt;*

*&lt;/SignedDoc&gt;*

*&lt;/dokument&gt;*<span id="anchor-411"></span>*&lt;/dokument&gt;*<span
id="anchor-412"></span>*&lt;/dokument&gt;*<span
id="anchor-411"></span>*&lt;/dokument&gt;*


Päringu receiveDocuments versioon v2 erineb eelmisest versioonist selle
poolest, et võimaldab dokumente DVK serverist alla laadida
fragmenteeritud kujul.

Päringu nimi: dhl.receiveDocuments.v2

Sisendi keha: Struct

integer arv

string\[\] kaust

string edastus\_id

integer fragment\_nr

long fragmendi\_suurus\_baitides

Väljundi keha: base64Binary

Element „arv“ määrab ära maksimaalse loetavate dokumentide arvu. Element
võib puududa, sellisel juhul tagastatakse vaikimisi 10 dokumenti.

Element „kaust“ määrab ära, millisest DVK kaustast dokumendid loetakse.
Element võib ka puududa (või olla väärtustamata), sellisel juhul
tagastatakse vaikimisi dokumendid kõigist kaustadest. Kui elemendiga
„kaust” on ette antud kaust, mida DVK-s ei eksisteeri, siis ei tagastata
ühtegi dokumenti.

Element „edastus\_id“ määrab edastussessiooni identifikaatori, kui
dokumente võetakse vastu tükkhaaval. Tükkhaaval saatmisel loeb DVK
server kõik sama „edastus\_id“ väärtusega tükid ühe ja sama
edastussessiooni andmete osadeks. Kui andmeid ei soovita vastu võtta
tükkhaaval, siis pole seda parameetrit vaja määrata.

Element „fragment\_nr“ määrab tükkhaaval vastuvõetavate andmete puhul,
mitmendat tükki klientrakendus järgmisena soovib serverilt saada
(loendamine algab 0-st). Kui andmeid ei soovita vastu võtta tükkhaaval,
siis pole seda parameetrit vaja määrata.

Element „fragmendi\_suurus\_baitides“ määrab tükkhaaval vastuvõetavate
andmete puhul, kui suurteks tükkideks peaks server kliendile saadetavad
andmed jaotama. Kui andmeid ei saadeta tükkhaaval, siis pole seda
parameetrit vaja määrata.

Väljund on base64 kodeeringus documentsArrayType tüüpi massiiv, mille
iga element on tagastatud dokument.

#### Näide

##### Päring

POST /cgi-bin/consumer\_proxy HTTP/1.0

Content-Type: text/xml

SOAPAction: ""

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;SOAP-ENV:Envelope

xmlns:SOAPENV="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;SOAP-ENV:Header&gt;

&lt;xtee:asutus xsi:type="xsd:string"&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu xsi:type="xsd:string"&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:isikukood
xsi:type="xsd:string"&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;xtee:id
xsi:type="xsd:string"&gt;6cae248568b3db7e97ff784673a4d38c5906bee0&lt;/xtee:id&gt;

&lt;xtee:nimi
xsi:type="xsd:string"&gt;dhl.receiveDocuments.v2&lt;/xtee:nimi&gt;

&lt;xtee:toimik xsi:type="xsd:string"&gt;&lt;/xtee:toimik&gt;

&lt;xtee:amet xsi:type="xsd:string"&gt;&lt;/xtee:amet&gt;

&lt;/SOAP-ENV:Header&gt;

&lt;SOAP-ENV:Body&gt;

&lt;dhl:receiveDocuments&gt;

&lt;keha&gt;

&lt;arv xsi:type="xsd:integer"&gt;10&lt;/arv&gt;

&lt;kaust/&gt;

&lt;/keha&gt;

&lt;/dhl:receiveDocuments&gt;

&lt;/SOAP-ENV:Body&gt;

&lt;/SOAP-ENV:Envelope&gt;

##### Päringu vastus

HTTP/1.1 200 OK

Content-Type: multipart/related; type=text/xml;

boundary="=\_9d665408f43f4698f71029c2df2b834e"

--=\_9d665408f43f4698f71029c2df2b834e

Content-Type:text/xml; charset="UTF-8"

Content-Transfer-Encoding:8bit

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;SOAP-ENV:Envelope xmlns:SOAPENV="

http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;SOAP-ENV:Header&gt;

&lt;xtee:asutus xsi:type="xsd:string"&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu xsi:type="xsd:string"&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:isikukood
xsi:type="xsd:string"&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;xtee:id

xsi:type="xsd:string"&gt;6cae248568b3db7e97ff784673a4d38c5906bee0&lt;/xtee:id&gt;

&lt;xtee:nimi
xsi:type="xsd:string"&gt;dhl.receiveDocuments.v2&lt;/xtee:nimi&gt;

&lt;xtee:toimik xsi:type="xsd:string"&gt;&lt;/xtee:toimik&gt;

&lt;xtee:amet xsi:type="xsd:string"&gt;&lt;/xtee:amet&gt;

&lt;/SOAP-ENV:Header&gt;

&lt;SOAP-ENV:Body&gt;

&lt;dhl:receiveDocumentsResponse&gt;

&lt;paring&gt;

&lt;arv xsi:type="xsd:integer"&gt;10&lt;/arv&gt;

&lt;kaust/&gt;

&lt;edastus\_id&gt;12345678\_1234&lt;/edastus\_id&gt;

&lt;fragment\_nr&gt;-1&lt;/fragment\_nr&gt;

&lt;fragmendi\_suurus\_baiides&gt;0&lt;/fragmendi\_suurus\_baitides&gt;

&lt;/paring&gt;

&lt;keha href="cid:793340a8216da081f3d785bcc74c0f74"/&gt;

&lt;/dhl:receiveDocumentsResponse&gt;

&lt;/SOAP-ENV:Body&gt;

&lt;/SOAP-ENV:Envelope&gt;

--=\_9d665408f43f4698f71029c2df2b834e

Content-Type:{http://www.w3.org/2001/XMLSchema}base64Binary

Content-Transfer-Encoding:base64

Content-Encoding: gzip

Content-ID:&lt;793340a8216da081f3d785bcc74c0f74&gt;

VMO1ZWxpbmUgdGFsdiB2w7VpYiBzYWFidWRhIGhpbGplbSBrdWkgdGF2YWxpc2Vs

dC4K

--=\_9d665408f43f4698f71029c2df2b834e

##### Päringu vastuse „keha“ elemendi sisu

Elemendi „keha“ sisu kodeerimata kujul on:

*&lt;dokument&gt;*

*&lt;metainfo&gt;*

*&lt;dhl\_id&gt;54365435&lt;/dhl\_id&gt;*

*&lt;/metainfo&gt;*

*&lt;transport&gt;*

*&lt;saatja&gt;*

*&lt;regnr&gt;00000000&lt;/regnr&gt;*

*&lt;nimi&gt;String&lt;/nimi&gt;*

*&lt;osakonna\_kood&gt;String&lt;/osakonna\_kood&gt;*

*&lt;osakonna\_nimi&gt;String&lt;/osakonna\_nimi&gt;*

*&lt;/saatja&gt;*

*&lt;saaja&gt;*

*&lt;regnr&gt;00000000&lt;/regnr&gt;*

*&lt;nimi&gt;String&lt;/nimi&gt;*

*&lt;osakonna\_kood&gt;String&lt;/osakonna\_kood&gt;*

*&lt;osakonna\_nimi&gt;String&lt;/osakonna\_nimi&gt;*

*&lt;/saaja&gt;*

*&lt;saaja&gt;*

*&lt;regnr&gt;00000000&lt;/regnr&gt;*

*&lt;nimi&gt;String&lt;/nimi&gt;*

*&lt;osakonna\_kood&gt;String&lt;/osakonna\_kood&gt;*

*&lt;osakonna\_nimi&gt;String&lt;/osakonna\_nimi&gt;*

*&lt;/saaja&gt;*

*&lt;/transport&gt;*

*&lt;ajalugu/&gt;*

*&lt;metaxml&gt;*

*&lt;p&gt;Siin sees võib olla suvaline XML jada&lt;/p&gt;*

*&lt;item&gt;Mingeid piiraguid elementidele pole&lt;/item&gt;*

*&lt;/metaxml&gt;*

*&lt;SignedDoc format="DIGIDOC-XML" version="1.3"
xmlns="http://www.sk.ee/DigiDoc/v1.3.0\#"&gt;*

*&lt;DataFile ContentType="EMBEDDED\_BASE64" Filename="dhl.xsd" Id="D0"
MimeType="text/xml" Size="3075"
xmlns="http://www.sk.ee/DigiDoc/v1.3.0\#"&gt;PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iaXNvLTg4NTktMSI/Pg0KPCEt*

*&lt;/DataFile&gt;*

*&lt;DataFile ContentType="EMBEDDED\_BASE64" Filename="dhl-aadress.xsd"
Id="D1" MimeType="text/xml" Size="2679"
xmlns="http://www.sk.ee/DigiDoc/v1.3.0\#"&gt;PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iaXNvLTg4NTktMSI/Pg0KPCEt*

*&lt;/DataFile&gt;*

*&lt;/SignedDoc&gt;*

*&lt;/dokument&gt;*


Päringu receiveDocuments versioon v3 erineb eelmisest versioonist selle
poolest, et võimaldab parameetritena ette anda allüksuse koodi ja
ametikoha koodi. See võimaldab vastu võtta ainult konkreetsele
allüksusele ja/või ametikohale adresseeritud dokumendid.

Päringu nimi: dhl.receiveDocuments.v3

Sisendi keha: Struct

integer arv

integer allyksus

integer ametikoht

string\[\] kaust

string edastus\_id

integer fragment\_nr

long fragmendi\_suurus\_baitides

Väljundi keha: base64Binary

Element „arv“ määrab ära maksimaalse loetavate dokumentide arvu. Element
võib puududa, sellisel juhul tagastatakse vaikimisi 10 dokumenti.

Element „allyksus“ määrab ära, millisele allüksusele adresseeritud
dokumente soovitakse vastu võtta. Parameetri väärtuseks saavad olla
DVK-s registreeritud allüksuste ID koodid. Kui lisaks parameetrile
„allyksus“ on väärtustatud ka parameeter „ametikoht“, siis võetakse
vastu ainult need dokumendid, mis on adresseeritud ühtaegu etteantud
allüksusele ja ametikohale.

Element „ametikoht“ määrab ära, millisele ametikohale adresseeritud
dokumente soovitakse vastu võtta. Parameetri väärtuseks saavad olla
DVK-s registreeritud ametikohtade ID koodid. Kui lisaks parameetrile
„ametikoht“ on väärtustatud ka parameeter „allyksus“, siis võetakse
vastu ainult need dokumendid, mis on adresseeritud ühtaegu etteantud
allüksusele ja ametikohale.

Element „kaust“ määrab ära, millisest DVK kaustast dokumendid loetakse.
Element võib ka puududa (või olla väärtustamata), sellisel juhul
tagastatakse vaikimisi dokumendid kõigist kaustadest. Kui elemendiga
„kaust” on ette antud kaust, mida DVK-s ei eksisteeri, siis ei tagastata
ühtegi dokumenti.

Element „edastus\_id“ määrab edastussessiooni identifikaatori, kui
dokumente võetakse vastu tükkhaaval. Tükkhaaval saatmisel loeb DVK
server kõik sama „edastus\_id“ väärtusega tükid ühe ja sama
edastussessiooni andmete osadeks. Kui andmeid ei soovita vastu võtta
tükkhaaval, siis pole seda parameetrit vaja määrata.

Element „fragment\_nr“ määrab tükkhaaval vastuvõetavate andmete puhul,
mitmendat tükki klientrakendus järgmisena soovib serverilt saada
(loendamine algab 0-st). Kui andmeid ei soovita vastu võtta tükkhaaval,
siis pole seda parameetrit vaja määrata.

Element „fragmendi\_suurus\_baitides“ määrab tükkhaaval vastuvõetavate
andmete puhul, kui suurteks tükkideks peaks server kliendile saadetavad
andmed jaotama. Kui andmeid ei saadeta tükkhaaval, siis pole seda
parameetrit vaja määrata.

Väljund on base64 kodeeringus documentsArrayType tüüpi massiiv, mille
iga element on tagastatud dokument.

#### Näide

##### Päring

POST /cgi-bin/consumer\_proxy HTTP/1.0

Content-Type: text/xml

SOAPAction: ""

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;SOAP-ENV:Envelope

xmlns:SOAPENV="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;SOAP-ENV:Header&gt;

&lt;xtee:asutus xsi:type="xsd:string"&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu xsi:type="xsd:string"&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:isikukood
xsi:type="xsd:string"&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;xtee:id
xsi:type="xsd:string"&gt;6cae248568b3db7e97ff784673a4d38c5906bee0&lt;/xtee:id&gt;

&lt;xtee:nimi
xsi:type="xsd:string"&gt;dhl.receiveDocuments.v3&lt;/xtee:nimi&gt;

&lt;xtee:toimik xsi:type="xsd:string"&gt;&lt;/xtee:toimik&gt;

&lt;xtee:amet xsi:type="xsd:string"&gt;&lt;/xtee:amet&gt;

&lt;/SOAP-ENV:Header&gt;

&lt;SOAP-ENV:Body&gt;

&lt;dhl:receiveDocuments&gt;

&lt;keha&gt;

&lt;arv xsi:type="xsd:integer"&gt;10&lt;/arv&gt;

&lt;kaust/&gt;

&lt;/keha&gt;

&lt;/dhl:receiveDocuments&gt;

&lt;/SOAP-ENV:Body&gt;

&lt;/SOAP-ENV:Envelope&gt;

##### Päringu vastus

HTTP/1.1 200 OK

Content-Type: multipart/related; type=text/xml;

boundary="=\_9d665408f43f4698f71029c2df2b834e"

--=\_9d665408f43f4698f71029c2df2b834e

Content-Type:text/xml; charset="UTF-8"

Content-Transfer-Encoding:8bit

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;SOAP-ENV:Envelope xmlns:SOAPENV="

http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;SOAP-ENV:Header&gt;

&lt;xtee:asutus xsi:type="xsd:string"&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu xsi:type="xsd:string"&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:isikukood
xsi:type="xsd:string"&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;xtee:id

xsi:type="xsd:string"&gt;6cae248568b3db7e97ff784673a4d38c5906bee0&lt;/xtee:id&gt;

&lt;xtee:nimi
xsi:type="xsd:string"&gt;dhl.receiveDocuments.v3&lt;/xtee:nimi&gt;

&lt;xtee:toimik xsi:type="xsd:string"&gt;&lt;/xtee:toimik&gt;

&lt;xtee:amet xsi:type="xsd:string"&gt;&lt;/xtee:amet&gt;

&lt;/SOAP-ENV:Header&gt;

&lt;SOAP-ENV:Body&gt;

&lt;dhl:receiveDocumentsResponse&gt;

&lt;paring&gt;

&lt;arv&gt;10&lt;/arv&gt;

&lt;allyksus&gt;1&lt;/allyksus&gt;

&lt;ametikoht&gt;0&lt;/ametikoht&gt;

&lt;kaust/&gt;

&lt;edastus\_id&gt;12345678\_1234&lt;/edastus\_id&gt;

&lt;fragment\_nr&gt;-1&lt;/fragment\_nr&gt;

&lt;fragmendi\_suurus\_baiides&gt;0&lt;/fragmendi\_suurus\_baitides&gt;

&lt;/paring&gt;

&lt;keha href="cid:793340a8216da081f3d785bcc74c0f74"/&gt;

&lt;/dhl:receiveDocumentsResponse&gt;

&lt;/SOAP-ENV:Body&gt;

&lt;/SOAP-ENV:Envelope&gt;

--=\_9d665408f43f4698f71029c2df2b834e

Content-Type:{http://www.w3.org/2001/XMLSchema}base64Binary

Content-Transfer-Encoding:base64

Content-Encoding: gzip

Content-ID:&lt;793340a8216da081f3d785bcc74c0f74&gt;

VMO1ZWxpbmUgdGFsdiB2w7VpYiBzYWFidWRhIGhpbGplbSBrdWkgdGF2YWxpc2Vs

dC4K

--=\_9d665408f43f4698f71029c2df2b834e

##### Päringu vastuse „keha“ elemendi sisu

Elemendi „keha“ sisu kodeerimata kujul on:

*&lt;dokument&gt;*

*&lt;metainfo&gt;*

*&lt;dhl\_id&gt;54365435&lt;/dhl\_id&gt;*

*&lt;/metainfo&gt;*

*&lt;transport&gt;*

*&lt;saatja&gt;*

*&lt;regnr&gt;00000000&lt;/regnr&gt;*

*&lt;nimi&gt;String&lt;/nimi&gt;*

*&lt;osakonna\_kood&gt;String&lt;/osakonna\_kood&gt;*

*&lt;osakonna\_nimi&gt;String&lt;/osakonna\_nimi&gt;*

*&lt;/saatja&gt;*

*&lt;saaja&gt;*

*&lt;regnr&gt;00000000&lt;/regnr&gt;*

*&lt;nimi&gt;String&lt;/nimi&gt;*

*&lt;allyksuse\_kood&gt;1&lt;/allyksuse\_kood&gt;*

*&lt;osakonna\_kood&gt;String&lt;/osakonna\_kood&gt;*

*&lt;osakonna\_nimi&gt;String&lt;/osakonna\_nimi&gt;*

*&lt;/saaja&gt;*

*&lt;/transport&gt;*

*&lt;ajalugu/&gt;*

*&lt;metaxml&gt;*

*&lt;p&gt;Siin sees võib olla suvaline XML jada&lt;/p&gt;*

*&lt;item&gt;Mingeid piiraguid elementidele pole&lt;/item&gt;*

*&lt;/metaxml&gt;*

*&lt;SignedDoc format="DIGIDOC-XML" version="1.3"
xmlns="http://www.sk.ee/DigiDoc/v1.3.0\#"&gt;*

*&lt;DataFile ContentType="EMBEDDED\_BASE64" Filename="dhl.xsd" Id="D0"
MimeType="text/xml" Size="3075"
xmlns="http://www.sk.ee/DigiDoc/v1.3.0\#"&gt;PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iaXNvLTg4NTktMSI/Pg0KPCEt*

*&lt;/DataFile&gt;*

*&lt;DataFile ContentType="EMBEDDED\_BASE64" Filename="dhl-aadress.xsd"
Id="D1" MimeType="text/xml" Size="2679"
xmlns="http://www.sk.ee/DigiDoc/v1.3.0\#"&gt;PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iaXNvLTg4NTktMSI/Pg0KPCEt*

*&lt;/DataFile&gt;*

*&lt;/SignedDoc&gt;*

*&lt;/dokument&gt;*


Päringu receiveDocuments versioon v4 erineb versioonist V3 selle
poolest, et võimaldab allüskuse ja ametikoha parameetritena kasutada
lühinimetusi (versioon V3 kasutas numbrilisi ID koode). See võimaldab
vastu võtta ainult konkreetsele allüksusele ja/või ametikohale
adresseeritud dokumente. Vastussõnumis olevad dokumendid kasutavad DVK
konteineri versioon 2.

Päringu nimi: dhl.receiveDocuments.v4

Sisendi keha: Struct

integer arv

string allyksuse\_lyhinimetus

string ametikoha\_lyhinimetus

string\[\] kaust

string edastus\_id

integer fragment\_nr

long fragmendi\_suurus\_baitides

Väljundi keha: base64Binary

Element „arv“ määrab ära maksimaalse loetavate dokumentide arvu. Element
võib puududa, sellisel juhul tagastatakse vaikimisi 10 dokumenti.

Element „allyksuse\_lyhinimetus“ määrab ära, millisele allüksusele
adresseeritud dokumente soovitakse vastu võtta. Parameetri väärtuseks
saavad olla teksti kujul allüksuste lühinimetused. Kui lisaks
parameetrile „allyksuse\_lyhinimetus“ on väärtustatud ka parameeter
„ametikoha\_lyhinimetus“, siis võetakse vastu ainult need dokumendid,
mis on adresseeritud ühtaegu etteantud allüksusele ja ametikohale.

Element „ametikoha\_lyhinimetus“ määrab ära, millisele ametikohale
adresseeritud dokumente soovitakse vastu võtta. Parameetri väärtuseks
saavad olla teksti kujul ametikohtade lühinimetused. Kui lisaks
parameetrile „ametikoha\_lyhinimetus“ on väärtustatud ka parameeter
„allyksuse\_lyhinimetus“, siis võetakse vastu ainult need dokumendid,
mis on adresseeritud ühtaegu etteantud allüksusele ja ametikohale.

Element „kaust“ määrab ära, millisest DVK kaustast dokumendid loetakse.
Element võib ka puududa (või olla väärtustamata), sellisel juhul
tagastatakse vaikimisi dokumendid kõigist kaustadest. Kui elemendiga
„kaust” on ette antud kaust, mida DVK-s ei eksisteeri, siis ei tagastata
ühtegi dokumenti.

Element „edastus\_id“ määrab edastussessiooni identifikaatori, kui
dokumente võetakse vastu tükkhaaval. Tükkhaaval saatmisel loeb DVK
server kõik sama „edastus\_id“ väärtusega tükid ühe ja sama
edastussessiooni andmete osadeks. Kui andmeid ei soovita vastu võtta
tükkhaaval, siis pole seda parameetrit vaja määrata.

Element „fragment\_nr“ määrab tükkhaaval vastuvõetavate andmete puhul,
mitmendat tükki klientrakendus järgmisena soovib serverilt saada
(loendamine algab 0-st). Kui andmeid ei soovita vastu võtta tükkhaaval,
siis pole seda parameetrit vaja määrata.

Element „fragmendi\_suurus\_baitides“ määrab tükkhaaval vastuvõetavate
andmete puhul, kui suurteks tükkideks peaks server kliendile saadetavad
andmed jaotama. Kui andmeid ei saadeta tükkhaaval, siis pole seda
parameetrit vaja määrata.

Väljund on base64 kodeeringus documentsArrayType tüüpi massiiv, mille
iga element on tagastatud dokument.

#### Näide

##### Päring

POST /cgi-bin/consumer\_proxy HTTP/1.0

Content-Type: text/xml

SOAPAction: ""

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;SOAP-ENV:Envelope

xmlns:SOAPENV="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;SOAP-ENV:Header&gt;

&lt;xtee:asutus xsi:type="xsd:string"&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu xsi:type="xsd:string"&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:isikukood
xsi:type="xsd:string"&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;xtee:id
xsi:type="xsd:string"&gt;6cae248568b3db7e97ff784673a4d38c5906bee0&lt;/xtee:id&gt;

&lt;xtee:nimi
xsi:type="xsd:string"&gt;dhl.receiveDocuments.v4&lt;/xtee:nimi&gt;

&lt;xtee:toimik xsi:type="xsd:string"&gt;&lt;/xtee:toimik&gt;

&lt;xtee:amet xsi:type="xsd:string"&gt;&lt;/xtee:amet&gt;

&lt;/SOAP-ENV:Header&gt;

&lt;SOAP-ENV:Body&gt;

&lt;dhl:receiveDocuments&gt;

&lt;keha&gt;

&lt;arv&gt;10&lt;/arv&gt;

&lt;allyksuse\_lyhinimetus&gt;RMTP&lt;/allyksuse\_lyhinimetus&gt;

&lt;ametikoha\_lyhinimetus&gt;RAAMATUPIDAJA&lt;/ametikoha\_lyhinimetus&gt;

&lt;kaust/&gt;

&lt;edastus\_id&gt;12345678\_1234&lt;/edastus\_id&gt;

&lt;fragment\_nr&gt;-1&lt;/fragment\_nr&gt;

&lt;fragmendi\_suurus\_baiides&gt;0&lt;/fragmendi\_suurus\_baitides&gt;

&lt;/keha&gt;

&lt;/dhl:receiveDocuments&gt;

&lt;/SOAP-ENV:Body&gt;

&lt;/SOAP-ENV:Envelope&gt;

##### Päringu vastus

HTTP/1.1 200 OK

Content-Type: multipart/related; type=text/xml;

boundary="=\_9d665408f43f4698f71029c2df2b834e"

--=\_9d665408f43f4698f71029c2df2b834e

Content-Type:text/xml; charset="UTF-8"

Content-Transfer-Encoding:8bit

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;SOAP-ENV:Envelope xmlns:SOAPENV="

http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;SOAP-ENV:Header&gt;

&lt;xtee:asutus xsi:type="xsd:string"&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu xsi:type="xsd:string"&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:isikukood
xsi:type="xsd:string"&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;xtee:id

xsi:type="xsd:string"&gt;6cae248568b3db7e97ff784673a4d38c5906bee0&lt;/xtee:id&gt;

&lt;xtee:nimi
xsi:type="xsd:string"&gt;dhl.receiveDocuments.v4&lt;/xtee:nimi&gt;

&lt;xtee:toimik xsi:type="xsd:string"&gt;&lt;/xtee:toimik&gt;

&lt;xtee:amet xsi:type="xsd:string"&gt;&lt;/xtee:amet&gt;

&lt;/SOAP-ENV:Header&gt;

&lt;SOAP-ENV:Body&gt;

&lt;dhl:receiveDocumentsResponse&gt;

&lt;paring&gt;

&lt;arv&gt;10&lt;/arv&gt;

&lt;allyksuse\_lyhinimetus&gt;RMTP&lt;/allyksuse\_lyhinimetus&gt;

&lt;ametikoha\_lyhinimetus&gt;RAAMATUPIDAJA&lt;/ametikoha\_lyhinimetus&gt;

&lt;kaust/&gt;

&lt;edastus\_id&gt;12345678\_1234&lt;/edastus\_id&gt;

&lt;fragment\_nr&gt;-1&lt;/fragment\_nr&gt;

&lt;fragmendi\_suurus\_baiides&gt;0&lt;/fragmendi\_suurus\_baitides&gt;

&lt;/paring&gt;

&lt;keha href="cid:793340a8216da081f3d785bcc74c0f74"/&gt;

&lt;/dhl:receiveDocumentsResponse&gt;

&lt;/SOAP-ENV:Body&gt;

&lt;/SOAP-ENV:Envelope&gt;

--=\_9d665408f43f4698f71029c2df2b834e

Content-Type:{http://www.w3.org/2001/XMLSchema}base64Binary

Content-Transfer-Encoding:base64

Content-Encoding: gzip

Content-ID:&lt;793340a8216da081f3d785bcc74c0f74&gt;

VMO1ZWxpbmUgdGFsdiB2w7VpYiBzYWFidWRhIGhpbGplbSBrdWkgdGF2YWxpc2Vs

dC4K

--=\_9d665408f43f4698f71029c2df2b834e

##### Päringu vastuse „keha“ elemendi sisu

Elemendi „keha“ sisu kodeerimata kujul on:

*&lt;dokument&gt;*

*&lt;metainfo&gt;*

*&lt;dhl\_id&gt;54365435&lt;/dhl\_id&gt;*

*&lt;/metainfo&gt;*

*&lt;transport&gt;*

*&lt;saatja&gt;*

*&lt;regnr&gt;00000000&lt;/regnr&gt;*

*&lt;nimi&gt;String&lt;/nimi&gt;*

*&lt;allyksuse\_lyhinimetus&gt;String&lt;/allyksuse\_lyhinimetus&gt;*

*&lt;allüksuse\_nimetus&gt;String&lt;/allüksuse\_nimetus&gt;*

*&lt;/saatja&gt;*

*&lt;saaja&gt;*

*&lt;regnr&gt;00000000&lt;/regnr&gt;*

*&lt;nimi&gt;String&lt;/nimi&gt;*

*&lt;allyksuse\_lyhinimetus&gt;String&lt;/allyksuse\_lyhinimetus&gt;*

*&lt;allüksuse\_nimetus&gt;String&lt;/allüksuse\_nimetus&gt;*

*&lt;/saaja&gt;*

*&lt;/transport&gt;*

*&lt;ajalugu/&gt;*

*&lt;metaxml&gt;*

*&lt;p&gt;Siin sees võib olla suvaline XML jada&lt;/p&gt;*

*&lt;item&gt;Mingeid piiraguid elementidele pole&lt;/item&gt;*

*&lt;/metaxml&gt;*

&lt;failid&gt;

&lt;kokku&gt;2&lt;/kokku&gt;

&lt;fail&gt;

&lt;jrknr&gt;1&lt;/jrknr&gt;

&lt;fail\_pealkiri&gt;DVK skeem&lt;/fail\_pealkiri&gt;

&lt;fail\_suurus&gt;3075&lt;/fail\_suurus&gt;

&lt;fail\_tyyp&gt;text/xml&lt;/fail\_tyyp&gt;

&lt;fail\_nimi&gt;dhl.xsd&lt;/fail\_nimi&gt;

&lt;zip\_base64\_sisu&gt;

*PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iaXNvLTg4NTktMSI/Pg0KPCEt
&lt;/zip\_base64\_sisu&gt;\
&lt;pohi\_dokument&gt;true&lt;/pohi\_dokument&gt;*

&lt;/fail&gt;

&lt;fail&gt;

&lt;jrknr&gt;2&lt;/jrknr&gt;

&lt;fail\_pealkiri&gt;DVK skeem&lt;/fail\_pealkiri&gt;

&lt;fail\_suurus&gt;2679&lt;/fail\_suurus&gt;

&lt;fail\_tyyp&gt;text/xml&lt;/fail\_tyyp&gt;

&lt;fail\_nimi&gt;dhl-aadress.xsd&lt;/fail\_nimi&gt;

&lt;zip\_base64\_sisu&gt;

*PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iaXNvLTg4NTktMSI/Pg0KPCEt
&lt;/zip\_base64\_sisu&gt;\
&lt;pohi\_dokument&gt;true&lt;/pohi\_dokument&gt;*

&lt;/fail&gt;

*&lt;/failid&gt;*

*&lt;/dokument&gt;*


Päringu nimi: dhl.markDocumentsReceived

Sisendi keha: Struct

base64Binary dokumendid

string kaust

Väljundi keha: string


Element „dokumendid“ on base64 kodeeringus documentRefsArrayType tüüpi
massiiv, mille iga element sisaldab loetud dokumendile DVK poolt
omistatud unikaalset ID-d. Kõik massiiviga antud dokumendid parameetriga
„kaust” näidatud kaustast märgitakse kättesaaduks.

Element „kaust“ väärtuseks saab olla DVK kausta täispikk nimi. Vaikimisi
märgitakse dokumendid kättesaaduks dokumendi ID põhjal sõltumata
sellest, millises kaustas ükski dokument asub.

Päring tagastab väljundi kehaks stringi sisuga „OK“.

#### <span id="anchor-473"></span>Näide

##### Päring

POST /cgi-bin/consumer\_proxy HTTP/1.0

Content-Type: multipart/related; type=text/xml;

boundary="=\_b5a8d09eeeb161be29def84633d6f6fc"

SOAPAction: ""

--=\_b5a8d09eeeb161be29def84633d6f6fc

Content-Type:text/xml; charset="UTF-8"

Content-Transfer-Encoding:8bit

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;SOAP-ENV:Envelope

xmlns:SOAPENV="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;SOAP-ENV:Header&gt;

&lt;xtee:asutus xsi:type="xsd:string"&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu xsi:type="xsd:string"&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:isikukood
xsi:type="xsd:string"&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;xtee:id
xsi:type="xsd:string"&gt;6cae248568b3db7e97ff784673a4d38c5906bee0&lt;/xtee:id&gt;

&lt;xtee:nimi
xsi:type="xsd:string"&gt;dhl.markDocumentsReceived.v1&lt;/xtee:nimi&gt;

&lt;xtee:toimik xsi:type="xsd:string"&gt;&lt;/xtee:toimik&gt;

&lt;xtee:amet xsi:type="xsd:string"&gt;&lt;/xtee:amet&gt;

&lt;/SOAP-ENV:Header&gt;

&lt;SOAP-ENV:Body&gt;

&lt;dhl: markDocumentsReceived&gt;

&lt;keha&gt;

&lt;dokumendid href="cid:b8fdc418df27ba3095a2d21b7be6d802"/&gt;

&lt;kaust/&gt;

&lt;/keha&gt;

&lt;/dhl: markDocumentsReceived&gt;

&lt;/SOAP-ENV:Body&gt;

&lt;/SOAP-ENV:Envelope&gt;

--=\_b5a8d09eeeb161be29def84633d6f6fc

Content-Disposition:php5hmCiX

Content-Type:{http://www.w3.org/2001/XMLSchema}base64Binary

Content-Transfer-Encoding:base64

Content-Encoding: gzip

Content-ID:&lt;b8fdc418df27ba3095a2d21b7be6d802&gt;

a29ybmkJa3cNCmtvcnNpa2EJY28NCmtyZWVrYQllbA0Ka3JpaQn1ZA0Ka3Vt9Wtp

…

dg0KbmVwYWxpCW5lDQpuaXZoaQnkdA0KbmphbmT+YQlueQ0Kbm9nYWkJ9ncNCg==

--=\_b5a8d09eeeb161be29def84633d6f6fc

##### Päringu vastus

HTTP/1.1 200 OK

Content-Type: text/xml

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;SOAP-ENV:Envelope xmlns:SOAPENV="

http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;SOAP-ENV:Header&gt;

&lt;xtee:asutus xsi:type="xsd:string"&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu xsi:type="xsd:string"&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:isikukood
xsi:type="xsd:string"&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;xtee:id

xsi:type="xsd:string"&gt;6cae248568b3db7e97ff784673a4d38c5906bee0&lt;/xtee:id&gt;

&lt;xtee:nimi
xsi:type="xsd:string"&gt;dhl.markDocumentsReceived.v1&lt;/xtee:nimi&gt;

&lt;xtee:toimik xsi:type="xsd:string"&gt;&lt;/xtee:toimik&gt;

&lt;xtee:amet xsi:type="xsd:string"&gt;&lt;/xtee:amet&gt;

&lt;/SOAP-ENV:Header&gt;

&lt;SOAP-ENV:Body&gt;

&lt;dhl:markDocumentsReceivedResponse&gt;

&lt;paring&gt;

&lt;dokumendid
xsi:type="xsd:string"&gt;52ed0ffbf27fc34759dce05d0e7bed2302876cec&lt;/dokumendid&gt;

&lt;kaust/&gt;

&lt;/paring&gt;

&lt;keha xsi:type="xsd:string"&gt;OK&lt;/keha&gt;

&lt;/dhl: markDocumentsReceivedResponse&gt;

&lt;/SOAP-ENV:Body&gt;

&lt;/SOAP-ENV:Envelope&gt;

##### Päringu „keha“ elemendi sisu

Elemendi „keha“ sisu kodeerimata kujul on:

*&lt;dhl\_id&gt;54365435&lt;/dhl\_id&gt;*


Element „dokumendid“ on base64 kodeeringus massiiv, mille iga element
„item” on alljärgneva struktuuriga:

-   „dhl\_id” element sisaldab loetud dokumendi DVK ID-d.
-   „vastuvotja\_staatus\_id” element sisaldab vastuvõtja poolel
    dokumendile omistatud staatuse identifikaatorit.
-   „fault” element sisaldab andmeid vastuvõtja poolel ilmnenud
    vea kohta.

    -   „faultcode” element sisaldab vea koodi.
    -   „faultactor” element sisaldab infot vea ilmnemise asukoha
        kohta (näit. kas viga ilmnes serveri või kliendi poolel)
    -   „faultstring” element sisaldab veateadet.
    -   „faultdetail” element sisaldab pikemat vea kirjeldust.
-   „metaxml” element sisaldab vaba struktuuriga XML- või tekstiandmed
    vastuvõetud dokumenti puudutavate andmete saatmiseks
    dokumendi saatjale.
-   „staatuse\_muutmise\_aeg“ sisaldab staatuse muutmise aega. Tegemist
    on mittekohustusliku parameetriga, mis võimaldab vajadusel
    täpsustada, et staatuse muutumise kuupäev oli päringu sooritamise
    hetkest varasem aeg. Kui antud parameetri väärtuseks anda tulevikus
    olev kuupäev, siis asendab DVK server selle päringu
    käivitamise ajaga.

Kõik massiiviga antud dokumendid parameetriga „kaust” näidatud kaustast
märgitakse kättesaaduks.

Element „kaust“ väärtuseks saab olla DVK kausta täispikk nimi. Vaikimisi
märgitakse dokumendid kättesaaduks dokumendi ID põhjal sõltumata
sellest, millises kaustas ükski dokument asub.

Päring tagastab väljundi kehaks stringi sisuga „OK“.

#### Näide

##### Päring

POST /cgi-bin/consumer\_proxy HTTP/1.0

Content-Type: multipart/related; type=text/xml;

boundary="=\_b5a8d09eeeb161be29def84633d6f6fc"

SOAPAction: ""

--=\_b5a8d09eeeb161be29def84633d6f6fc

Content-Type:text/xml; charset="UTF-8"

Content-Transfer-Encoding:8bit

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;SOAP-ENV:Envelope

xmlns:SOAPENV="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;SOAP-ENV:Header&gt;

&lt;xtee:asutus xsi:type="xsd:string"&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu xsi:type="xsd:string"&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:isikukood
xsi:type="xsd:string"&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;xtee:id
xsi:type="xsd:string"&gt;6cae248568b3db7e97ff784673a4d38c5906bee0&lt;/xtee:id&gt;

&lt;xtee:nimi
xsi:type="xsd:string"&gt;dhl.markDocumentsReceived.v2&lt;/xtee:nimi&gt;

&lt;xtee:toimik xsi:type="xsd:string"&gt;&lt;/xtee:toimik&gt;

&lt;xtee:amet xsi:type="xsd:string"&gt;&lt;/xtee:amet&gt;

&lt;/SOAP-ENV:Header&gt;

&lt;SOAP-ENV:Body&gt;

&lt;dhl: markDocumentsReceived&gt;

&lt;keha&gt;

&lt;dokumendid href="cid:b8fdc418df27ba3095a2d21b7be6d802"/&gt;

&lt;kaust/&gt;

&lt;/keha&gt;

&lt;/dhl: markDocumentsReceived&gt;

&lt;/SOAP-ENV:Body&gt;

&lt;/SOAP-ENV:Envelope&gt;

--=\_b5a8d09eeeb161be29def84633d6f6fc

Content-Disposition:php5hmCiX

Content-Type:{http://www.w3.org/2001/XMLSchema}base64Binary

Content-Transfer-Encoding:base64

Content-Encoding: gzip

Content-ID:&lt;b8fdc418df27ba3095a2d21b7be6d802&gt;

a29ybmkJa3cNCmtvcnNpa2EJY28NCmtyZWVrYQllbA0Ka3JpaQn1ZA0Ka3Vt9Wtp

…

dg0KbmVwYWxpCW5lDQpuaXZoaQnkdA0KbmphbmT+YQlueQ0Kbm9nYWkJ9ncNCg==

--=\_b5a8d09eeeb161be29def84633d6f6fc

##### Päringu vastus

HTTP/1.1 200 OK

Content-Type: text/xml

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;SOAP-ENV:Envelope xmlns:SOAPENV="

http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;SOAP-ENV:Header&gt;

&lt;xtee:asutus xsi:type="xsd:string"&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu xsi:type="xsd:string"&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:isikukood
xsi:type="xsd:string"&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;xtee:id

xsi:type="xsd:string"&gt;6cae248568b3db7e97ff784673a4d38c5906bee0&lt;/xtee:id&gt;

&lt;xtee:nimi
xsi:type="xsd:string"&gt;dhl.markDocumentsReceived.v2&lt;/xtee:nimi&gt;

&lt;xtee:toimik xsi:type="xsd:string"&gt;&lt;/xtee:toimik&gt;

&lt;xtee:amet xsi:type="xsd:string"&gt;&lt;/xtee:amet&gt;

&lt;/SOAP-ENV:Header&gt;

&lt;SOAP-ENV:Body&gt;

&lt;dhl:markDocumentsReceivedResponse&gt;

&lt;paring&gt;

&lt;dokumendid
xsi:type="xsd:string"&gt;52ed0ffbf27fc34759dce05d0e7bed2302876cec&lt;/dokumendid&gt;

&lt;kaust/&gt;

&lt;/paring&gt;

&lt;keha xsi:type="xsd:string"&gt;OK&lt;/keha&gt;

&lt;/dhl: markDocumentsReceivedResponse&gt;

&lt;/SOAP-ENV:Body&gt;

&lt;/SOAP-ENV:Envelope&gt;

##### Päringu „keha“ elemendi sisu

Elemendi „keha“ sisu kodeerimata kujul on:

*&lt;item&gt;*

*&lt;dhl\_id&gt;54365435&lt;/dhl\_id&gt;*

*&lt;vastuvotja\_staatus\_id&gt;10&lt;/vastuvotja\_staatus\_id&gt;*

*&lt;fault&gt;*

*&lt;faultcode&gt;100&lt;/faultcode&gt;*

*&lt;faultactor&gt;CLIENT&lt;/faultactor&gt;*

*&lt;faultstring&gt;Veateade&lt;/faultstring&gt;*

*&lt;faultdetail&gt;vea kirjeldus&lt;/faultdetail&gt;*

*&lt;/fault&gt;*

*&lt;metaxml&gt;*

*&lt;andmed\_1&gt;väärtus 1&lt;/andmed\_1&gt;*

*&lt;andmed\_2&gt;väärtus 2&lt;/andmed\_2&gt;*

*&lt;andmed\_n&gt;väärtus n&lt;/andmed\_n&gt;*

*&lt;/metaxml&gt;*

*&lt;staatuse\_muutmise\_aeg&gt;2005-09-01T12:35:11&lt;/staatuse\_muutmise\_aeg&gt;*

*&lt;/item&gt;*

*&lt;item&gt;*

*&lt;dhl\_id&gt;54365436&lt;/dhl\_id&gt;*

*&lt;vastuvotja\_staatus\_id&gt;7&lt;/vastuvotja\_staatus\_id&gt;*

*&lt;staatuse\_muutmise\_aeg&gt;2005-09-02T14:15:00&lt;/staatuse\_muutmise\_aeg&gt;*

*&lt;/item&gt;*

*&lt;item&gt;*

*&lt;dhl\_id&gt;54365437&lt;/dhl\_id&gt;*

*&lt;/item&gt;*


Päringu markDogumentsReceived versioon v3 eelneb varasematest
versioonidest selle poolest, et elemendi „dokumendid“ sisu asub nüüd
SOAP sõnumi kehas (varasemates versioonides asus base64 kodeeritud kujul
SOAP sõnumi manuses). Samuti on lisatud võimalus märkida dokumendid
vastuvõetuks kasutades dokumendi GUID tüüpi identifikaatorit (sellisel
juhul asendab element &lt;dokument\_guid&gt; elemendi &lt;dhl\_id&gt;).

Element „dokumendid“ on SOAP massiiv, mille iga element „item” on
alljärgneva struktuuriga:

-   „dhl\_id” element sisaldab loetud dokumendi DVK ID-d.
-   „vastuvotja\_staatus\_id” element sisaldab vastuvõtja poolel
    dokumendile omistatud staatuse identifikaatorit.
-   „fault” element sisaldab andmeid vastuvõtja poolel ilmnenud
    vea kohta.

    -   „faultcode” element sisaldab vea koodi.
    -   „faultactor” element sisaldab infot vea ilmnemise asukoha
        kohta (näit. kas viga ilmnes serveri või kliendi poolel)
    -   „faultstring” element sisaldab veateadet.
    -   „faultdetail” element sisaldab pikemat vea kirjeldust.
-   „metaxml” element sisaldab vaba struktuuriga XML- või tekstiandmed
    vastuvõetud dokumenti puudutavate andmete saatmiseks
    dokumendi saatjale.
-   „staatuse\_muutmise\_aeg“ sisaldab staatuse muutmise aega. Tegemist
    on mittekohustusliku parameetriga, mis võimaldab vajadusel
    täpsustada, et staatuse muutumise kuupäev oli päringu sooritamise
    hetkest varasem aeg. Kui antud parameetri väärtuseks anda tulevikus
    olev kuupäev, siis asendab DVK server selle päringu
    käivitamise ajaga.

Kõik massiiviga antud dokumendid parameetriga „kaust” näidatud kaustast
märgitakse kättesaaduks.

Element „kaust“ väärtuseks saab olla DVK kausta täispikk nimi. Vaikimisi
märgitakse dokumendid kättesaaduks dokumendi ID põhjal sõltumata
sellest, millises kaustas ükski dokument asub.

Päring tagastab väljundi kehaks stringi sisuga „OK“.

#### Näide

##### Päring

POST /cgi-bin/consumer\_proxy HTTP/1.0

Content-Type: multipart/related; type=text/xml;

boundary="=\_b5a8d09eeeb161be29def84633d6f6fc"

SOAPAction: ""

--=\_b5a8d09eeeb161be29def84633d6f6fc

Content-Type:text/xml; charset="UTF-8"

Content-Transfer-Encoding:8bit

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;SOAP-ENV:Envelope

xmlns:SOAPENV="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;SOAP-ENV:Header&gt;

&lt;xtee:asutus xsi:type="xsd:string"&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu xsi:type="xsd:string"&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:isikukood
xsi:type="xsd:string"&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;xtee:id
xsi:type="xsd:string"&gt;6cae248568b3db7e97ff784673a4d38c5906bee0&lt;/xtee:id&gt;

&lt;xtee:nimi
xsi:type="xsd:string"&gt;dhl.markDocumentsReceived.v3&lt;/xtee:nimi&gt;

&lt;xtee:toimik xsi:type="xsd:string"&gt;&lt;/xtee:toimik&gt;

&lt;xtee:amet xsi:type="xsd:string"&gt;&lt;/xtee:amet&gt;

&lt;/SOAP-ENV:Header&gt;

&lt;SOAP-ENV:Body&gt;

&lt;dhl:markDocumentsReceived&gt;

&lt;keha&gt;

&lt;dokumendid xsi:type="SOAP-ENC:Array"
SOAP-ENC:arrayType="dhl:asutus\[3\]"&gt;

&lt;item&gt;

&lt;dhl\_id&gt;54365435&lt;/dhl\_id&gt;

&lt;vastuvotja\_staatus\_id&gt;10&lt;/vastuvotja\_staatus\_id&gt;

&lt;fault&gt;

&lt;faultcode&gt;100&lt;/faultcode&gt;

&lt;faultactor&gt;CLIENT&lt;/faultactor&gt;

&lt;faultstring&gt;Veateade&lt;/faultstring&gt;

&lt;faultdetail&gt;vea kirjeldus&lt;/faultdetail&gt;

&lt;/fault&gt;

&lt;metaxml&gt;

&lt;andmed\_1&gt;väärtus 1&lt;/andmed\_1&gt;

&lt;andmed\_2&gt;väärtus 2&lt;/andmed\_2&gt;

&lt;andmed\_n&gt;väärtus n&lt;/andmed\_n&gt;

&lt;/metaxml&gt;

&lt;staatuse\_muutmise\_aeg&gt;2005-09-01T12:35:11&lt;/staatuse\_muutmise\_aeg&gt;

&lt;/item&gt;

&lt;item&gt;

&lt;dhl\_id&gt;54365436&lt;/dhl\_id&gt;

&lt;vastuvotja\_staatus\_id&gt;7&lt;/vastuvotja\_staatus\_id&gt;

&lt;staatuse\_muutmise\_aeg&gt;2005-09-02T14:15:00&lt;/staatuse\_muutmise\_aeg&gt;

&lt;/item&gt;

&lt;item&gt;

&lt;dhl\_id&gt;54365437&lt;/dhl\_id&gt;

&lt;/item&gt;

&lt;/dokumendid&gt;

&lt;kaust/&gt;

&lt;/keha&gt;

&lt;/dhl:markDocumentsReceived&gt;

&lt;/SOAP-ENV:Body&gt;

&lt;/SOAP-ENV:Envelope&gt;

##### Päringu vastus

HTTP/1.1 200 OK

Content-Type: text/xml

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;SOAP-ENV:Envelope xmlns:SOAPENV="

http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;SOAP-ENV:Header&gt;

&lt;xtee:asutus xsi:type="xsd:string"&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu xsi:type="xsd:string"&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:isikukood
xsi:type="xsd:string"&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;xtee:id

xsi:type="xsd:string"&gt;6cae248568b3db7e97ff784673a4d38c5906bee0&lt;/xtee:id&gt;

&lt;xtee:nimi
xsi:type="xsd:string"&gt;dhl.markDocumentsReceived.v2&lt;/xtee:nimi&gt;

&lt;xtee:toimik xsi:type="xsd:string"&gt;&lt;/xtee:toimik&gt;

&lt;xtee:amet xsi:type="xsd:string"&gt;&lt;/xtee:amet&gt;

&lt;/SOAP-ENV:Header&gt;

&lt;SOAP-ENV:Body&gt;

&lt;dhl:markDocumentsReceivedResponse&gt;

&lt;paring&gt;

&lt;dokumendid xsi:type="SOAP-ENC:Array"
SOAP-ENC:arrayType="dhl:asutus\[3\]"&gt;

&lt;item&gt;

&lt;dhl\_id&gt;54365435&lt;/dhl\_id&gt;

&lt;vastuvotja\_staatus\_id&gt;10&lt;/vastuvotja\_staatus\_id&gt;

&lt;fault&gt;

&lt;faultcode&gt;100&lt;/faultcode&gt;

&lt;faultactor&gt;CLIENT&lt;/faultactor&gt;

&lt;faultstring&gt;Veateade&lt;/faultstring&gt;

&lt;faultdetail&gt;vea kirjeldus&lt;/faultdetail&gt;

&lt;/fault&gt;

&lt;metaxml&gt;

&lt;andmed\_1&gt;väärtus 1&lt;/andmed\_1&gt;

&lt;andmed\_2&gt;väärtus 2&lt;/andmed\_2&gt;

&lt;andmed\_n&gt;väärtus n&lt;/andmed\_n&gt;

&lt;/metaxml&gt;

&lt;staatuse\_muutmise\_aeg&gt;2005-09-01T12:35:11&lt;/staatuse\_muutmise\_aeg&gt;

&lt;/item&gt;

&lt;item&gt;

&lt;dhl\_id&gt;54365436&lt;/dhl\_id&gt;

&lt;vastuvotja\_staatus\_id&gt;7&lt;/vastuvotja\_staatus\_id&gt;

&lt;staatuse\_muutmise\_aeg&gt;2005-09-02T14:15:00&lt;/staatuse\_muutmise\_aeg&gt;

&lt;/item&gt;

&lt;item&gt;

&lt;dhl\_id&gt;54365437&lt;/dhl\_id&gt;

&lt;/item&gt;

&lt;/dokumendid&gt;

&lt;kaust/&gt;

&lt;/paring&gt;

&lt;keha xsi:type="xsd:string"&gt;OK&lt;/keha&gt;

&lt;/dhl:markDocumentsReceivedResponse&gt;

&lt;/SOAP-ENV:Body&gt;

&lt;/SOAP-ENV:Envelope&gt;



Päringu nimi: dhl.getSendingOptions.v1

Sisendi keha: stringide massiiv

Väljundi keha: massiiv andmetüübist „asutus”:

regnrstring

nimistring

saatminemassiiv

saatmisviisstring(väärtused: dhl | dhl\_otse)

ks\_asutuse\_regnrstring(kõrgemalseisva asutuse registrikood)

Päring annab teada, kas ja kui siis millisel moel on asutused võimelised
DVK-d kasutama. Ilma sisendparameetriteta käivitamise korral tagastab
päring nimekirja kõigist DVK-ga liitunud asutustest. Kui aga päringule
anda sisendparameetriks nimekiri asutuste registrikoodidest, tagastab
päring info nimekirjas sisaldunud asutuste võimekuse kohta DVK kaudu
andmeid vahetada.

Antud päringu puhul esitatakse nii sisend- kui väljundandmed pakkimata
ja kodeerimata kujul.

#### Näide

##### Päring

POST /cgi-bin/consumer\_proxy HTTP/1.0

Content-Type: text/xml; charset=utf-8

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"&gt;

&lt;env:Header&gt;

&lt;xtee:asutus&gt;11181967&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:ametnik&gt;38005130332&lt;/xtee:ametnik&gt;

&lt;xtee:nimi&gt;dhl.getSendingOptions.v1&lt;/xtee:nimi&gt;

&lt;xtee:id&gt;dhl\_client\_1&lt;/xtee:id&gt;

&lt;xtee:toimik/&gt;

&lt;xtee:isikukood&gt;EE38005130332&lt;/xtee:isikukood&gt;

&lt;/env:Header&gt;

&lt;env:Body&gt;

&lt;dhl:getSendingOptions&gt;

&lt;keha&gt;

&lt;asutus&gt;12345678&lt;/asutus&gt;

&lt;asutus&gt;23456789&lt;/asutus&gt;

&lt;asutus&gt;34567890&lt;/asutus&gt;

&lt;/keha&gt;

&lt;/dhl:getSendingOptions&gt;

&lt;/env:Body&gt;

&lt;/env:Envelope&gt;

##### Päringu vastus

HTTP/1.1 200 OK

SOAPAction: ""

Content-Type: text/xml

&lt;?xml version="1.0" encoding="utf-8"?&gt;

&lt;soapenv:Envelope
xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:dhl="http://www.riik.ee/schemas/dhl"&gt;

&lt;env:Header&gt;

&lt;xtee:asutus&gt;11181967&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:ametnik&gt;38005130332&lt;/xtee:ametnik&gt;

&lt;xtee:nimi&gt;dhl.getSendingOptions.v1&lt;/xtee:nimi&gt;

&lt;xtee:id&gt;dhl\_client\_1&lt;/xtee:id&gt;

&lt;xtee:toimik/&gt;

&lt;xtee:isikukood&gt;EE38005130332&lt;/xtee:isikukood&gt;

&lt;/env:Header&gt;

&lt;soapenv:Body&gt;

&lt;dhl:getSendingOptionsResponse&gt;

&lt;paring xsi:type="SOAP-ENC:Array"
SOAP-ENC:arrayType="xsd:string\[3\]"&gt;

&lt;asutus&gt;12345678&lt;/asutus&gt;

&lt;asutus&gt;23456789&lt;/asutus&gt;

&lt;asutus&gt;34567890&lt;/asutus&gt;

&lt;/paring&gt;

&lt;keha xsi:type="SOAP-ENC:Array"
SOAP-ENC:arrayType="dhl:asutus\[3\]"&gt;

&lt;asutus&gt;

&lt;regnr&gt;12345678&lt;/regnr&gt;

&lt;nimi&gt;ASUTUS 1&lt;/nimi&gt;

&lt;saatmine&gt;

&lt;saatmisviis&gt;dhl&lt;/saatmisviis&gt;

&lt;/saatmine&gt;

&lt;ks\_asutuse\_regnr/&gt;

&lt;/asutus&gt;

&lt;asutus&gt;

&lt;regnr&gt;23456789&lt;/regnr&gt;

&lt;nimi&gt;ASUTUS 2&lt;/nimi&gt;

&lt;saatmine&gt;

&lt;saatmisviis&gt;dhl&lt;/saatmisviis&gt;

&lt;saatmisviis&gt;dhl\_otse&lt;/saatmisviis&gt;

&lt;/saatmine&gt;

&lt;ks\_asutuse\_regnr&gt;12345678&lt;/ks\_asutuse\_regnr&gt;

&lt;/asutus&gt;

&lt;asutus&gt;

&lt;regnr&gt;34567890&lt;/regnr&gt;

&lt;nimi/&gt;

&lt;saatmine/&gt;

&lt;ks\_asutuse\_regnr/&gt;

&lt;/asutus&gt;

&lt;/keha&gt;

&lt;/dhl:getSendingOptionsResponse&gt;

&lt;/soapenv:Body&gt;

&lt;/soapenv:Envelope&gt;


Päringu nimi: dhl.getSendingOptions.v2

Sisendi keha: andmestruktuur:

asutusedstringide massiiv

vahetatud\_dokumente\_vahemaltnumber

vahetatud\_dokumente\_kuninumber

vastuvotmata\_dokumente\_ooteltõeväärtus (jah/ei)

Väljundi keha: massiiv andmetüübist „asutus”:

regnrstring

nimistring

saatminemassiiv

saatmisviisstring(väärtused: dhl | dhl\_otse)

ks\_asutuse\_regnrstring(kõrgemalseisva asutuse registrikood)

Päring annab teada, kas ja kui siis millisel moel on asutused võimelised
DVK-d kasutama. Ilma sisendparameetriteta käivitamise korral tagastab
päring nimekirja kõigist DVK-ga liitunud asutustest.

Päringu parameetrite otstarve on järgmine:

-   **asutused**\
    Võimaldab ette anda nimekirja asutuste registrikoodidest. DVK
    tagastab vastuses info ainult nimekirjas loetletud asutuste
    DVK-võimekuse kohta. Käesoleva parameetriga etteantud nimekiri
    toimib täiendava filtrina ka järgmiste parameetrite puhul.
-   **vahetatud\_dokumente\_vahemalt**\
    DVK tagastab nimekirja asutustest, mis on vahetanud vähemalt
    etteantud arvu (kaasa arvatud) dokumente.
-   **vahetatud\_dokumente\_kuni**\
    DVK tagastab nimekirja asutustest, mis on vahetanud kuni etteantud
    arvu (kaasa arvatud) dokumente.
-   **vastuvotmata\_dokumente\_ootel**\
    Kui antud parameetri väärtus on true, siis tagastab DVK nimekirja
    asutustest, millel on hetkel allalaadimist ootavaid dokumente.

Antud päringu puhul esitatakse nii sisend- kui väljundandmed pakkimata
ja kodeerimata kujul.

#### Näide

##### Päring

POST /cgi-bin/consumer\_proxy HTTP/1.0

Content-Type: text/xml; charset=utf-8

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"&gt;

&lt;env:Header&gt;

&lt;xtee:asutus&gt;11181967&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:ametnik&gt;38005130332&lt;/xtee:ametnik&gt;

&lt;xtee:nimi&gt;dhl.getSendingOptions.v2&lt;/xtee:nimi&gt;

&lt;xtee:id&gt;dhl\_client\_1&lt;/xtee:id&gt;

&lt;xtee:toimik/&gt;

&lt;xtee:isikukood&gt;EE38005130332&lt;/xtee:isikukood&gt;

&lt;/env:Header&gt;

&lt;env:Body&gt;

&lt;dhl:getSendingOptions&gt;

&lt;keha&gt;

&lt;asutused&gt;

&lt;asutus&gt;12345678&lt;/asutus&gt;

&lt;asutus&gt;23456789&lt;/asutus&gt;

&lt;asutus&gt;34567890&lt;/asutus&gt;

&lt;/asutused&gt;

&lt;vahetatud\_dokumente\_vahemalt&gt;0&lt;/vahetatud\_dokumente\_vahemalt&gt;

&lt;vahetatud\_dokumente\_kuni&gt;1000&lt;/vahetatud\_dokumente\_kuni&gt;

&lt;vastuvotmata\_dokumente\_ootel&gt;true&lt;/vastuvotmata\_dokumente\_ootel&gt;

&lt;/keha&gt;

&lt;/dhl:getSendingOptions&gt;

&lt;/env:Body&gt;

&lt;/env:Envelope&gt;

##### Päringu vastus

HTTP/1.1 200 OK

SOAPAction: ""

Content-Type: text/xml

&lt;?xml version="1.0" encoding="utf-8"?&gt;

&lt;soapenv:Envelope
xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:dhl="http://www.riik.ee/schemas/dhl"&gt;

&lt;env:Header&gt;

&lt;xtee:asutus&gt;11181967&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:ametnik&gt;38005130332&lt;/xtee:ametnik&gt;

&lt;xtee:nimi&gt;dhl.getSendingOptions.v2&lt;/xtee:nimi&gt;

&lt;xtee:id&gt;dhl\_client\_1&lt;/xtee:id&gt;

&lt;xtee:toimik/&gt;

&lt;xtee:isikukood&gt;EE38005130332&lt;/xtee:isikukood&gt;

&lt;/env:Header&gt;

&lt;soapenv:Body&gt;

&lt;dhl:getSendingOptionsResponse&gt;

&lt;paring xsi:type="SOAP-ENC:Array"
SOAP-ENC:arrayType="xsd:string\[3\]"&gt;

&lt;asutus&gt;12345678&lt;/asutus&gt;

&lt;asutus&gt;23456789&lt;/asutus&gt;

&lt;asutus&gt;34567890&lt;/asutus&gt;

&lt;/paring&gt;

&lt;keha xsi:type="SOAP-ENC:Array"
SOAP-ENC:arrayType="dhl:asutus\[3\]"&gt;

&lt;asutus&gt;

&lt;regnr&gt;12345678&lt;/regnr&gt;

&lt;nimi&gt;ASUTUS 1&lt;/nimi&gt;

&lt;saatmine&gt;

&lt;saatmisviis&gt;dhl&lt;/saatmisviis&gt;

&lt;/saatmine&gt;

&lt;ks\_asutuse\_regnr/&gt;

&lt;/asutus&gt;

&lt;asutus&gt;

&lt;regnr&gt;23456789&lt;/regnr&gt;

&lt;nimi&gt;ASUTUS 2&lt;/nimi&gt;

&lt;saatmine&gt;

&lt;saatmisviis&gt;dhl&lt;/saatmisviis&gt;

&lt;saatmisviis&gt;dhl\_otse&lt;/saatmisviis&gt;

&lt;/saatmine&gt;

&lt;ks\_asutuse\_regnr&gt;12345678&lt;/ks\_asutuse\_regnr&gt;

&lt;/asutus&gt;

&lt;asutus&gt;

&lt;regnr&gt;34567890&lt;/regnr&gt;

&lt;nimi/&gt;

&lt;saatmine/&gt;

&lt;ks\_asutuse\_regnr/&gt;

&lt;/asutus&gt;

&lt;/keha&gt;

&lt;/dhl:getSendingOptionsResponse&gt;

&lt;/soapenv:Body&gt;

&lt;/soapenv:Envelope&gt;


Päringu nimi:dhl.getSendingOptions.v3

Sisendi keha:base64Binary

Sisendi keha kodeerimata kujul:

kehaStruct

asutusedmassiiv

asutusstring

allyksusedmassiiv

asutuse\_koodstring

lyhinimetusstring

ametikohadmassiiv

asutuse\_koodstring

lyhinimetusstring

vahetatud\_dokumente\_vahemaltnumber

vahetatud\_dokumente\_kuninumber

vastuvotmata\_dokumente\_ooteltõeväärtus (jah/ei)

Väljundi keha:base64Binary

Väljundi keha kodeerimata kujul:

kehaStruct

asutusedmassiiv

asutusStruct

regnrstring

nimistring

saatminemassiiv

saatmisviisstring(väärtused: dhl | dhl\_otse)

ks\_asutuse\_regnrstring(kõrgemalseisva asutuse registrikood)

allyksusedmassiiv

allyksusStruct

koodstring

nimetusstring

asutuse\_koodstring

lyhinimetusstring

ks\_allyksuse\_lyhinimetusstring

ametikohadmassiiv

ametikohtStruct

koodstring

nimetusstring

asutuse\_koodstring

lyhinimetusstring

ks\_allyksuse\_lyhinimetusstring

Päring annab teada, kas ja kui siis millisel moel on asutused võimelised
DVK-d kasutama. Ilma sisendparameetriteta käivitamise korral tagastab
päring nimekirja kõigist DVK-ga liitunud asutustest.

Päringu parameetrite otstarve on järgmine:

-   **asutused**\
    Võimaldab ette anda nimekirja asutuste registrikoodidest. DVK
    tagastab vastuses info ainult nimekirjas loetletud asutuste
    DVK-võimekuse kohta. Käesoleva parameetriga etteantud nimekiri
    toimib täiendava filtrina ka parameetrite
    „vahetatud\_dokumente\_vahemalt“, „vahetatud\_dokumente\_kuni“ ja
    „vastuvotmata\_dokumente\_ootel“ puhul.
-   **allyksused**\
    Võimaldab ette anda nimekirja asutuste allüksustest. Allüksuste
    nimekiri esitatakse asutuse registrikoodi ja allüksuse
    lühinime paaridena. DVK tagastab vastuses info ainult nimekirjas
    loetletud allüksuste kohta. Käesoleva parameetriga etteantud
    nimekiri toimib täiendava filtrina ka parameetrite
    „vahetatud\_dokumente\_vahemalt“, „vahetatud\_dokumente\_kuni“ ja
    „vastuvotmata\_dokumente\_ootel“ puhul.
-   **ametikohad**\
    Võimaldab ette anda nimekirja asutuste ametikohtadest. Ametikohtade
    nimekiri esitatakse asutuse registrikoodi ja ametikoha
    lühinime paaridena. DVK tagastab vastuses info ainult nimekirjas
    loetletud ametikohtade kohta. Käesoleva parameetriga etteantud
    nimekiri toimib täiendava filtrina ka parameetrite
    „vahetatud\_dokumente\_vahemalt“, „vahetatud\_dokumente\_kuni“ ja
    „vastuvotmata\_dokumente\_ootel“ puhul.
-   **vahetatud\_dokumente\_vahemalt**\
    DVK tagastab nimekirja asutustest, allüksustest ja ametikohtadest,
    mis on vahetanud vähemalt etteantud arvu (kaasa arvatud) dokumente.
-   **vahetatud\_dokumente\_kuni**\
    DVK tagastab nimekirja asutustest, allüksustest ja ametikohtadest,
    mis on vahetanud kuni etteantud arvu (kaasa arvatud) dokumente.
-   **vastuvotmata\_dokumente\_ootel**\
    Kui antud parameetri väärtus on true, siis tagastab DVK nimekirja
    asutustest, allüksustest ja ametikohtadest, millel on hetkel
    allalaadimist ootavaid dokumente.

#### Näide

##### Päring

POST /cgi-bin/consumer\_proxy HTTP/1.0

Content-Type: text/xml; charset=utf-8

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"&gt;

&lt;env:Header&gt;

&lt;xtee:asutus&gt;11181967&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:ametnik&gt;38005130332&lt;/xtee:ametnik&gt;

&lt;xtee:nimi&gt;dhl.getSendingOptions.v3&lt;/xtee:nimi&gt;

&lt;xtee:id&gt;dhl\_client\_1&lt;/xtee:id&gt;

&lt;xtee:toimik/&gt;

&lt;xtee:isikukood&gt;EE38005130332&lt;/xtee:isikukood&gt;

&lt;/env:Header&gt;

&lt;env:Body&gt;

&lt;dhl:getSendingOptions&gt;

&lt;keha href="cid:1266238098953"/&gt;

&lt;/dhl:getSendingOptions&gt;

&lt;/env:Body&gt;

&lt;/env:Envelope&gt;

------=\_Part\_0\_32961147.1266238098984

Content-Type: {http://www.w3.org/2001/XMLSchema}base64Binary

Content-Transfer-Encoding: binary

Content-Id: &lt;1266238098953&gt;

Content-Encoding: gzip

H4sIAAAAAAAAAH2PSwrDMAxEj2T670L4BoVSujcCC2psR8WWArl9mpAEl0J3w/DmCcEbC2YiKcFb6LGK9iw

...

ZDbzrTXjHtuyN3z7yv/AAAAA==

------=\_Part\_0\_32961147.1266238098984—

##### Päringu vastus

HTTP/1.1 200 OK

SOAPAction: ""

Content-Type: text/xml

&lt;?xml version="1.0" encoding="utf-8"?&gt;

&lt;soapenv:Envelope
xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:dhl="http://www.riik.ee/schemas/dhl"&gt;

&lt;env:Header&gt;

&lt;xtee:asutus&gt;11181967&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:ametnik&gt;38005130332&lt;/xtee:ametnik&gt;

&lt;xtee:nimi&gt;dhl.getSendingOptions.v3&lt;/xtee:nimi&gt;

&lt;xtee:id&gt;dhl\_client\_1&lt;/xtee:id&gt;

&lt;xtee:toimik/&gt;

&lt;xtee:isikukood&gt;EE38005130332&lt;/xtee:isikukood&gt;

&lt;/env:Header&gt;

&lt;soapenv:Body&gt;

&lt;dhl:getSendingOptionsResponse&gt;

&lt;paring&gt;7EC4A81438DC484DE15A2DD2D371323B&lt;/paring&gt;

&lt;keha href="cid:514E5DB49902EC1EC1BCBA442FF37A02"/&gt;

&lt;/dhl:getSendingOptionsResponse&gt;

&lt;/soapenv:Body&gt;

&lt;/soapenv:Envelope&gt;

------=\_Part\_2\_33268061.1266238099109

Content-Type: {http://www.w3.org/2001/XMLSchema}base64Binary

Content-Transfer-Encoding: binary

Content-Id: &lt;514E5DB49902EC1EC1BCBA442FF37A02&gt;

Content-Encoding: gzip

H4sIAAAAAAAAAG2P3QrCMAyFHyk4fy9CYaAX4oWCD1AKLa6s68ayCXt7287OVr1qzslp8gUFjcNISjL8rhj

...

W9+AXNEgoqqAQAA

------=\_Part\_2\_33268061.1266238099109--

##### Päringu „keha“ elemendi sisu

Elemendi „keha“ sisu kodeerimata kujul on:

*&lt;keha&gt;*

*&lt;asutused&gt;*

*&lt;asutus&gt;12345678&lt;/asutus&gt;*

*&lt;asutus&gt;23456789&lt;/asutus&gt;*

*&lt;asutus&gt;34567890&lt;/asutus&gt;*

*&lt;/asutused&gt;*

*&lt;allyksused&gt;*

*&lt;allyksus&gt;*

*&lt;asutuse\_kood&gt;12345678&lt;/asutuse\_kood&gt;*

*&lt;lyhinimetus&gt;RMTP&lt;/lyhinimetus&gt;*

*&lt;/allyksus&gt;*

*&lt;allyksus&gt;*

*&lt;asutuse\_kood&gt;23456789&lt;/asutuse\_kood&gt;*

*&lt;lyhinimetus&gt;DH&lt;/lyhinimetus&gt;*

*&lt;/allyksus&gt;*

*&lt;allyksus&gt;*

*&lt;asutuse\_kood&gt;23456789&lt;/asutuse\_kood&gt;*

*&lt;lyhinimetus&gt;YLD&lt;/lyhinimetus&gt;*

*&lt;/allyksus&gt;*

*&lt;/allyksused&gt;*

*&lt;ametikohad&gt;*

*&lt;ametikoht&gt;*

*&lt;asutuse\_kood&gt;12345678&lt;/asutuse\_kood&gt;*

*&lt;lyhinimetus&gt;PEARAAMAT&lt;/lyhinimetus&gt;*

*&lt;/ametikoht&gt;*

*&lt;ametikoht&gt;*

*&lt;asutuse\_kood&gt;12345678&lt;/asutuse\_kood&gt;*

*&lt;lyhinimetus&gt;RAAMATUPIDAJA&lt;/lyhinimetus&gt;*

*&lt;/ametikoht&gt;*

*&lt;/ametikohad&gt;*

*&lt;vahetatud\_dokumente\_vahemalt&gt;0&lt;/vahetatud\_dokumente\_vahemalt&gt;*

*&lt;vahetatud\_dokumente\_kuni&gt;1000&lt;/vahetatud\_dokumente\_kuni&gt;*

*&lt;vastuvotmata\_dokumente\_ootel&gt;true&lt;/vastuvotmata\_dokumente\_ootel&gt;*

*&lt;/keha&gt;*

##### Vastuse „keha“ elemendi sisu

Elemendi „keha“ sisu kodeerimata kujul on:

*&lt;keha&gt;*

*&lt;asutused&gt;*

*&lt;asutus&gt;*

*&lt;regnr&gt;12345678&lt;/regnr&gt;*

*&lt;nimi&gt;ASUTUS 1&lt;/nimi&gt;*

*&lt;saatmine&gt;*

*&lt;saatmisviis&gt;dhl&lt;/saatmisviis&gt;*

*&lt;/saatmine&gt;*

*&lt;ks\_asutuse\_regnr/&gt;*

*&lt;/asutus&gt;*

*&lt;asutus&gt;*

*&lt;regnr&gt;23456789&lt;/regnr&gt;*

*&lt;nimi&gt;ASUTUS 2&lt;/nimi&gt;*

*&lt;saatmine&gt;*

*&lt;saatmisviis&gt;dhl&lt;/saatmisviis&gt;*

*&lt;saatmisviis&gt;dhl\_otse&lt;/saatmisviis&gt;*

*&lt;/saatmine&gt;*

*&lt;ks\_asutuse\_regnr&gt;12345678&lt;/ks\_asutuse\_regnr&gt;*

*&lt;/asutus&gt;*

*&lt;asutus&gt;*

*&lt;regnr&gt;34567890&lt;/regnr&gt;*

*&lt;nimi/&gt;*

*&lt;saatmine/&gt;*

*&lt;ks\_asutuse\_regnr/&gt;*

*&lt;/asutus&gt;*

*&lt;/asutused&gt;*

*&lt;allyksused&gt;*

*&lt;allyksus&gt;*

*&lt;kood&gt;47&lt;/kood&gt;*

*&lt;nimetus&gt;Raamatupidamine&lt;/nimetus&gt;*

*&lt;asutuse\_kood&gt;12345678&lt;/asutuse\_kood&gt;*

*&lt;lyhinimetus&gt;RMTP&lt;/lyhinimetus&gt;*

*&lt;ks\_allyksuse\_lyhinimetus&gt;YLD&lt;/ks\_allyksuse\_lyhinimetus&gt;*

*&lt;/allyksus&gt;*

*&lt;allyksus&gt;*

*&lt;kood&gt;17&lt;/kood&gt;*

*&lt;nimetus&gt;Üldosakond&lt;/nimetus&gt;*

*&lt;asutuse\_kood&gt;23456789&lt;/asutuse\_kood&gt;*

*&lt;lyhinimetus&gt;YLD&lt;/lyhinimetus&gt;*

*&lt;ks\_allyksuse\_lyhinimetus/&gt;*

*&lt;/allyksus&gt;*

*&lt;/allyksused&gt;*

*&lt;ametikohad&gt;*

*&lt;ametikoht&gt;*

*&lt;kood&gt;92&lt;/kood&gt;*

*&lt;nimetus&gt;Raamatupidaja&lt;/nimetus&gt;*

*&lt;asutuse\_kood&gt;12345678&lt;/asutuse\_kood&gt;*

*&lt;lyhinimetus&gt;RAAMATUPIDAJA&lt;/lyhinimetus&gt;*

*&lt;ks\_allyksuse\_lyhinimetus&gt;RMTP&lt;/ks\_allyksuse\_lyhinimetus&gt;*

*&lt;/ametikoht&gt;*

*&lt;/ametikohad&gt;*

*&lt;/keha&gt;*


Päringu nimi: dhl.changeOrganizationData.v1

Sisendi keha: Struct:

string registrikood

string endine\_registrikood

string korgemalseisva\_asutuse\_registrikood

string nimi

string nime\_lyhend

string liik1

string liik2

string tegevusala

string tegevuspiirkond

string maakond

string asukoht

string aadress

string postikood

string telefon

string faks

string e\_post

string www

string logo

date asutamise\_kuupaev

string moodustamise\_akti\_nimi

string moodustamise\_akti\_number

date moodustamise\_akti\_kuupaev

string pohimaaruse\_akti\_nimi

string pohimaaruse\_akti\_number

date pohimaaruse\_kinnitamise\_kuupaev

date pohimaaruse\_kande\_kuupaev

string parameetrid

boolean dvk\_saatmine

boolean dvk\_otse\_saatmine

string toetatav\_dvk\_versioon

string dokumendihaldussysteemi\_nimetus

Väljundi keha: string

Päringuga saab uuendada DVK serveri asutuste registris salvestatud
andmeid asutuse kohta. Päringuga saab uuendada ainult selle asutuse
andmeid, mille nimel päring käivitati (s.t. mille registrikood oli
märgitud X-Tee päringu päistesse).

Kui andmete uuendamine õnnestub, siis tagastab päring vastussõnumi kehas
väärtuse „OK“

Antud päringu puhul esitatakse nii sisend- kui väljundandmed pakkimata
ja kodeerimata kujul.


##### Päring

POST /cgi-bin/consumer\_proxy HTTP/1.0

Content-Type: text/xml; charset=utf-8

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"&gt;

&lt;env:Header&gt;

&lt;xtee:asutus&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:ametnik&gt;12345678901&lt;/xtee:ametnik&gt;

&lt;xtee:nimi&gt;dhl.changeOrganizationData.v1&lt;/xtee:nimi&gt;

&lt;xtee:id&gt;dhl\_client\_1&lt;/xtee:id&gt;

&lt;xtee:toimik/&gt;

&lt;xtee:isikukood&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;/env:Header&gt;

&lt;env:Body&gt;

&lt;dhl:changeOrganizationData&gt;

&lt;keha&gt;

&lt;registrikood&gt;12345678&lt;/registrikood&gt;

&lt;endine\_registrikood&gt;12345&lt;/endine\_registrikood&gt;

&lt;korgemalseisva\_asutuse\_registrikood&gt;12322332
&lt;/korgemalseisva\_asutuse\_registrikood&gt;

&lt;nimi&gt;Asutus AS&lt;/nimi&gt;

&lt;nime\_lyhend&gt;AAS&lt;/nime\_lyhend&gt;

&lt;liik1&gt;L1&lt;/liik1&gt;

&lt;liik2&gt;L2&lt;/liik2&gt;

&lt;tegevusala&gt;Kaubandus&lt;/tegevusala&gt;

&lt;tegevuspiirkond&gt;Tallinn&lt;/tegevuspiirkond&gt;

&lt;maakond&gt;Harjumaa&lt;/maakond&gt;

&lt;asukoht&gt;Tallinn&lt;/asukoht&gt;

&lt;aadress&gt;Akadeemia tee 21&lt;/aadress&gt;

&lt;postikood&gt;12618&lt;/postikood&gt;

&lt;telefon&gt;6543210&lt;/telefon&gt;

&lt;faks&gt;6543211&lt;/faks&gt;

&lt;e\_post&gt;info@asutus.ee&lt;/e\_post&gt;

&lt;www&gt;http://www.asutus.ee&lt;/www&gt;

&lt;logo/&gt;

&lt;asutamise\_kuupaev&gt;2008-01-11&lt;/asutamise\_kuupaev&gt;

&lt;moodustamise\_akti\_nimi&gt;Moodustamise
akt&lt;/moodustamise\_akti\_nimi&gt;

&lt;moodustamise\_akti\_number&gt;1A2&lt;/moodustamise\_akti\_number&gt;

&lt;moodustamise\_akti\_kuupaev&gt;2008-02-01&lt;/moodustamise\_akti\_kuupaev&gt;

&lt;pohimaaruse\_akti\_nimi&gt;Põhimäärus&lt;/pohimaaruse\_akti\_nimi&gt;

&lt;pohimaaruse\_akti\_number&gt;2A3&lt;/pohimaaruse\_akti\_number&gt;

&lt;pohimaaruse\_kinnitamise\_kuupaev&gt;2008-03-01
&lt;/pohimaaruse\_kinnitamise\_kuupaev&gt;

&lt;pohimaaruse\_kande\_kuupaev&gt;2008-03-05&lt;/pohimaaruse\_kande\_kuupaev&gt;

&lt;parameetrid&gt;Parameetrid 123&lt;/parameetrid&gt;

&lt;dvk\_saatmine&gt;1&lt;/dvk\_saatmine&gt;

&lt;dvk\_otse\_saatmine&gt;0&lt;/dvk\_otse\_saatmine&gt;

&lt;toetatav\_dvk\_versioon&gt;1.5&lt;/toetatav\_dvk\_versioon&gt;

&lt;dokumendihaldussysteemi\_nimetus&gt;SuperDoc 2000
&lt;/dokumendihaldussysteemi\_nimetus&gt;

&lt;/keha&gt;

&lt;/dhl:changeOrganizationData&gt;

&lt;/env:Body&gt;

&lt;/env:Envelope&gt;

##### Päringu vastus

HTTP/1.1 200 OK

SOAPAction: ""

Content-Type: text/xml

&lt;?xml version="1.0" encoding="utf-8"?&gt;

&lt;soapenv:Envelope
xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:dhl="http://www.riik.ee/schemas/dhl"&gt;

&lt;env:Header&gt;

&lt;xtee:asutus&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:ametnik&gt;12345678901&lt;/xtee:ametnik&gt;

&lt;xtee:nimi&gt;dhl.changeOrganizationData.v2&lt;/xtee:nimi&gt;

&lt;xtee:id&gt;dhl\_client\_1&lt;/xtee:id&gt;

&lt;xtee:toimik/&gt;

&lt;xtee:isikukood&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;/env:Header&gt;

&lt;soapenv:Body&gt;

&lt;dhl:changeOrganizationDataResponse&gt;

&lt;paring xsi:type="SOAP-ENC:Array"
SOAP-ENC:arrayType="xsd:string\[3\]"&gt;

&lt;registrikood&gt;12345678&lt;/registrikood&gt;

&lt;endine\_registrikood&gt;12345&lt;/endine\_registrikood&gt;

&lt;korgemalseisva\_asutuse\_registrikood&gt;12322332
&lt;/korgemalseisva\_asutuse\_registrikood&gt;

&lt;nimi&gt;Asutus AS&lt;/nimi&gt;

&lt;nime\_lyhend&gt;AAS&lt;/nime\_lyhend&gt;

&lt;liik1&gt;L1&lt;/liik1&gt;

&lt;liik2&gt;L2&lt;/liik2&gt;

&lt;tegevusala&gt;Kaubandus&lt;/tegevusala&gt;

&lt;tegevuspiirkond&gt;Tallinn&lt;/tegevuspiirkond&gt;

&lt;maakond&gt;Harjumaa&lt;/maakond&gt;

&lt;asukoht&gt;Tallinn&lt;/asukoht&gt;

&lt;aadress&gt;Akadeemia tee 21&lt;/aadress&gt;

&lt;postikood&gt;12618&lt;/postikood&gt;

&lt;telefon&gt;6543210&lt;/telefon&gt;

&lt;faks&gt;6543211&lt;/faks&gt;

&lt;e\_post&gt;info@asutus.ee&lt;/e\_post&gt;

&lt;www&gt;http://www.asutus.ee&lt;/www&gt;

&lt;logo/&gt;

&lt;asutamise\_kuupaev&gt;2008-01-11&lt;/asutamise\_kuupaev&gt;

&lt;moodustamise\_akti\_nimi&gt;Moodustamise
akt&lt;/moodustamise\_akti\_nimi&gt;

&lt;moodustamise\_akti\_number&gt;1A2&lt;/moodustamise\_akti\_number&gt;

&lt;moodustamise\_akti\_kuupaev&gt;2008-02-01&lt;/moodustamise\_akti\_kuupaev&gt;

&lt;pohimaaruse\_akti\_nimi&gt;Põhimäärus&lt;/pohimaaruse\_akti\_nimi&gt;

&lt;pohimaaruse\_akti\_number&gt;2A3&lt;/pohimaaruse\_akti\_number&gt;

&lt;pohimaaruse\_kinnitamise\_kuupaev&gt;2008-03-01
&lt;/pohimaaruse\_kinnitamise\_kuupaev&gt;

&lt;pohimaaruse\_kande\_kuupaev&gt;2008-03-05&lt;/pohimaaruse\_kande\_kuupaev&gt;

&lt;parameetrid&gt;Parameetrid 123&lt;/parameetrid&gt;

&lt;dvk\_saatmine&gt;1&lt;/dvk\_saatmine&gt;

&lt;dvk\_otse\_saatmine&gt;0&lt;/dvk\_otse\_saatmine&gt;

&lt;toetatav\_dvk\_versioon&gt;1.5&lt;/toetatav\_dvk\_versioon&gt;

&lt;dokumendihaldussysteemi\_nimetus&gt;SuperDoc 2000
&lt;/dokumendihaldussysteemi\_nimetus&gt;

&lt;/paring&gt;

&lt;keha&gt;OK&lt;/keha&gt;

&lt;/dhl:changeOrganizationDataResponse&gt;

&lt;/soapenv:Body&gt;

&lt;/soapenv:Envelope&gt;


Päringu nimi: dhl.deleteOldDocuments.v1

Sisendi keha: -

Väljundi keha: string

Päring kustutab DVK andmebaasist säilitustähtaja ületanud dokumendid.
Kui säilitustähtaja ületanud dokument on osadele adressaatidele veel
saatmata, siis märgib DVK server dokumendi andmetesse, et dokumendi
edastamine on katkestatud ning lükkab dokumendi säilitustähtaega
mõnevõrra edasi (sõltub serveri seadistusest), et info saatmise
katkestamisest jõuaks kindlasti ka dokumendi saatjale.

Kui säilitustähtaja ületanud dokumentide kustutamine õnnestub, siis
tagastab päring vastussõnumi kehas väärtuse „OK“.


##### Päring

POST /cgi-bin/consumer\_proxy HTTP/1.0

Content-Type: text/xml; charset=utf-8

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"&gt;

&lt;env:Header&gt;

&lt;xtee:asutus&gt;11181967&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:ametnik&gt;38005130332&lt;/xtee:ametnik&gt;

&lt;xtee:nimi&gt;dhl.deleteOldDocuments.v1&lt;/xtee:nimi&gt;

&lt;xtee:id&gt;dhl\_client\_1&lt;/xtee:id&gt;

&lt;xtee:toimik/&gt;

&lt;xtee:isikukood&gt;EE38005130332&lt;/xtee:isikukood&gt;

&lt;/env:Header&gt;

&lt;env:Body&gt;

&lt;dhl:deleteOldDocuments&gt;

&lt;keha/&gt;

&lt;/dhl:deleteOldDocuments&gt;

&lt;/env:Body&gt;

&lt;/env:Envelope&gt;

##### Päringu vastus

HTTP/1.1 200 OK

SOAPAction: ""

Content-Type: text/xml

&lt;?xml version="1.0" encoding="utf-8"?&gt;

&lt;soapenv:Envelope
xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:dhl="http://www.riik.ee/schemas/dhl"&gt;

&lt;env:Header&gt;

&lt;xtee:asutus&gt;11181967&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:ametnik&gt;38005130332&lt;/xtee:ametnik&gt;

&lt;xtee:nimi&gt;dhl.deleteOldDocuments.v1&lt;/xtee:nimi&gt;

&lt;xtee:id&gt;dhl\_client\_1&lt;/xtee:id&gt;

&lt;xtee:toimik/&gt;

&lt;xtee:isikukood&gt;EE38005130332&lt;/xtee:isikukood&gt;

&lt;/env:Header&gt;

&lt;soapenv:Body&gt;

&lt;dhl:deleteOldDocumentsResponse&gt;

&lt;paring/&gt;

&lt;keha&gt;OK&lt;/keha&gt;

&lt;/dhl:deleteOldDocumentsResponse&gt;

&lt;/soapenv:Body&gt;

&lt;/soapenv:Envelope&gt;


Päringu nimi: dhl.runSystemCheck.v1

Sisendi keha: -

Väljundi keha: string

Päring kontrollib DVK serveri kriitiliste funktsioonide toimimist
(andmebaasi ligipääs, kettale kirjutamine, jne.). Kui kõik
kontrollitavad funktsioonid toimivad, siis tagastab päring väärtuse
„OK“. Avastatud vea korral tagastab päring veateate SOAP veateate kujul.


##### Päring

POST /cgi-bin/consumer\_proxy HTTP/1.0

Content-Type: text/xml; charset=utf-8

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"&gt;

&lt;env:Header&gt;

&lt;xtee:asutus&gt;11181967&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:ametnik&gt;38005130332&lt;/xtee:ametnik&gt;

&lt;xtee:nimi&gt;dhl.runSystemCheck.v1&lt;/xtee:nimi&gt;

&lt;xtee:id&gt;dhl\_client\_1&lt;/xtee:id&gt;

&lt;xtee:toimik/&gt;

&lt;xtee:isikukood&gt;EE38005130332&lt;/xtee:isikukood&gt;

&lt;/env:Header&gt;

&lt;env:Body&gt;

&lt;dhl:runSystemCheck&gt;

&lt;keha/&gt;

&lt;/dhl:runSystemCheck&gt;

&lt;/env:Body&gt;

&lt;/env:Envelope&gt;

##### Päringu vastus

HTTP/1.1 200 OK

SOAPAction: ""

Content-Type: text/xml

&lt;?xml version="1.0" encoding="utf-8"?&gt;

&lt;soapenv:Envelope
xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:dhl="http://www.riik.ee/schemas/dhl"&gt;

&lt;env:Header&gt;

&lt;xtee:asutus&gt;11181967&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:ametnik&gt;38005130332&lt;/xtee:ametnik&gt;

&lt;xtee:nimi&gt;dhl.runSystemCheck.v1&lt;/xtee:nimi&gt;

&lt;xtee:id&gt;dhl\_client\_1&lt;/xtee:id&gt;

&lt;xtee:toimik/&gt;

&lt;xtee:isikukood&gt;EE38005130332&lt;/xtee:isikukood&gt;

&lt;/env:Header&gt;

&lt;soapenv:Body&gt;

&lt;dhl:runSystemCheckResponse&gt;

&lt;paring/&gt;

&lt;keha&gt;OK&lt;/keha&gt;

&lt;/dhl:runSystemCheckResponse&gt;

&lt;/soapenv:Body&gt;

&lt;/soapenv:Envelope&gt;


Päringu nimi: dhl.getSubdivisionList.v1

Sisendi keha: stringide massiiv

Väljundi keha: massiiv andmetüübist „allyksus”:

koodstring

nimetusstring

asutuse\_koodstring

lyhinimetusstring

ks\_allyksuse\_lyhinimetusstring(kõrgemalseisva allüksuse lühinimetus)

Päring tagastab sisendparameetriga etteantud asutuste allüksuste
nimekirja. Päring on vajalik selleks, et oleks võimalik dokumente
adresseerida asutuse allüksusele (et oleks teada, millisele allüksusele
DVK serveris milline unikaalne kood vastab).

Antud päringu puhul esitatakse nii sisend- kui väljundandmed pakkimata
ja kodeerimata kujul.

#### Näide

##### Päring

POST /cgi-bin/consumer\_proxy HTTP/1.0

Content-Type: text/xml; charset=utf-8

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;env:Header&gt;

&lt;xtee:asutus&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:ametnik&gt;12345678901&lt;/xtee:ametnik&gt;

&lt;xtee:nimi&gt;dhl.getSubdivisionList.v1&lt;/xtee:nimi&gt;

&lt;xtee:id&gt;dhl\_client\_1&lt;/xtee:id&gt;

&lt;xtee:toimik/&gt;

&lt;xtee:isikukood&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;/env:Header&gt;

&lt;env:Body&gt;

&lt;dhl:getSubdivisionList&gt;

&lt;keha xsi:type="SOAP-ENC:Array"
SOAP-ENC:arrayType="xsd:string\[3\]"&gt;

&lt;asutus&gt;12345678&lt;/asutus&gt;

&lt;asutus&gt;23456789&lt;/asutus&gt;

&lt;asutus&gt;34567890&lt;/asutus&gt;

&lt;/keha&gt;

&lt;/dhl:getSubdivisionList&gt;

&lt;/env:Body&gt;

&lt;/env:Envelope&gt;

##### Päringu vastus

HTTP/1.1 200 OK

SOAPAction: ""

Content-Type: text/xml

&lt;?xml version="1.0" encoding="utf-8"?&gt;

&lt;soapenv:Envelope
xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:dhl="http://www.riik.ee/schemas/dhl"&gt;

&lt;env:Header&gt;

&lt;xtee:asutus&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:ametnik&gt;12345678901&lt;/xtee:ametnik&gt;

&lt;xtee:nimi&gt;dhl.getSubdivisionList.v1&lt;/xtee:nimi&gt;

&lt;xtee:id&gt;dhl\_client\_1&lt;/xtee:id&gt;

&lt;xtee:toimik/&gt;

&lt;xtee:isikukood&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;/env:Header&gt;

&lt;soapenv:Body&gt;

&lt;dhl:getSubdivisionListResponse&gt;

&lt;paring xsi:type="SOAP-ENC:Array"
SOAP-ENC:arrayType="xsd:string\[3\]"&gt;

&lt;asutus&gt;12345678&lt;/asutus&gt;

&lt;asutus&gt;23456789&lt;/asutus&gt;

&lt;asutus&gt;34567890&lt;/asutus&gt;

&lt;/paring&gt;

&lt;keha xsi:type="SOAP-ENC:Array"
SOAP-ENC:arrayType="dhl:allyksus\[4\]"&gt;

&lt;allyksus&gt;

&lt;kood&gt;1&lt;/kood&gt;

&lt;nimetus&gt;IT osakond&lt;/nimetus&gt;

&lt;asutuse\_kood&gt;12345678&lt;/asutuse\_kood&gt;

&lt;lyhinimetus&gt;IT&lt;/lyhinimetus&gt;

&lt;ks\_allyksuse\_lyhinimetus&gt;YLD&lt;/ks\_allyksuse\_lyhinimetus&gt;

&lt;/allyksus&gt;

&lt;allyksus&gt;

&lt;kood&gt;2&lt;/kood&gt;

&lt;nimetus&gt;Üldosakond&lt;/nimetus&gt;

&lt;asutuse\_kood&gt;12345678&lt;/asutuse\_kood&gt;

&lt;lyhinimetus&gt;YLD&lt;/lyhinimetus&gt;

&lt;ks\_allyksuse\_lyhinimetus/&gt;

&lt;/allyksus&gt;

&lt;allyksus&gt;

&lt;kood&gt;12&lt;/kood&gt;

&lt;nimetus&gt;Raamatupidamisosakond&lt;/nimetus&gt;

&lt;asutuse\_kood&gt;23456789&lt;/asutuse\_kood&gt;

&lt;lyhinimetus&gt;RMTP&lt;/lyhinimetus&gt;

&lt;ks\_allyksuse\_lyhinimetus/&gt;

&lt;/allyksus&gt;

&lt;allyksus&gt;

&lt;kood&gt;28&lt;/kood&gt;

&lt;nimetus&gt;Haldusosakond&lt;/nimetus&gt;

&lt;asutuse\_kood&gt;34567890&lt;/asutuse\_kood&gt;

&lt;lyhinimetus&gt;haldus&lt;/lyhinimetus&gt;

&lt;ks\_allyksuse\_lyhinimetus/&gt;

&lt;/allyksus&gt;

&lt;/keha&gt;

&lt;/dhl:getSubdivisionListResponse&gt;

&lt;/soapenv:Body&gt;

&lt;/soapenv:Envelope&gt;


Päringu getSubdivisionList versioon v2 eelneb varasematest versioonidest
selle poolest, et päringu ja vastuse andmed asuvad nüüd SOAP sõnumi
manustes (varasemates versioonides asusid andmed SOAP sõnumi kehas).

Päringu nimi: dhl.getSubdivisionList.v2

Sisendi keha:

asutusedstringide massiiv päringu MIME manuses base64 kujul

Väljundi keha:

allyksusedmassiiv andmetüübist allyksus päringu MIME manuses base64
kujul

koodstring

nimetusstring

asutuse\_koodstring

lyhinimetusstring

ks\_allyksuse\_lyhinimetusstring(kõrgemalseisva allüksuse lühinimetus)

Päring tagastab sisendparameetriga etteantud asutuste allüksuste
nimekirja. Päring on vajalik selleks, et oleks võimalik dokumente
adresseerida asutuse allüksusele (et oleks teada, millisele allüksusele
DVK serveris milline unikaalne kood vastab).

Antud päringu sisend- ja väljundandmed saadetakse SOAP sõnumi MIME
manustes. Enne SOAP sõnumi manusesse paigutamist pakitakse XML gzip-ga
kokku ja kodeeritakse base64 kujule.

#### Näide

##### Päring

POST /cgi-bin/consumer\_proxy HTTP/1.0

Content-Type: text/xml; charset=utf-8

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"&gt;

&lt;env:Header&gt;

&lt;xtee:asutus&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:ametnik&gt;12345678901&lt;/xtee:ametnik&gt;

&lt;xtee:nimi&gt;dhl.getSubdivisionList.v2&lt;/xtee:nimi&gt;

&lt;xtee:id&gt;dhl\_client\_1&lt;/xtee:id&gt;

&lt;xtee:toimik/&gt;

&lt;xtee:isikukood&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;/env:Header&gt;

&lt;env:Body&gt;

&lt;dhl:getSubdivisionList&gt;

&lt;keha&gt;

&lt;asutused href="cid:1265809289328"/&gt;

&lt;/keha&gt;

&lt;/dhl:getSubdivisionList&gt;

&lt;/env:Body&gt;

&lt;/env:Envelope&gt;

------=\_Part\_1\_16862753.1265809289328

Content-Type: {http://www.w3.org/2001/XMLSchema}base64Binary

Content-Transfer-Encoding: binary

Content-Id: &lt;1265809289328&gt;

Content-Encoding: gzip

H4sIAAAAAAAAALNJLC4tKS1OTbGzgbDsLMzNTE2MjQxt9KECMAlDI2MTUzNzC4SEPlwzAG7f6Q9HAAAA

------=\_Part\_1\_16862753.1265809289328—

##### Päringu vastus

HTTP/1.1 200 OK

SOAPAction: ""

Content-Type: text/xml

&lt;?xml version="1.0" encoding="utf-8"?&gt;

&lt;soapenv:Envelope
xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:dhl="http://www.riik.ee/schemas/dhl"&gt;

&lt;env:Header&gt;

&lt;xtee:asutus&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:ametnik&gt;12345678901&lt;/xtee:ametnik&gt;

&lt;xtee:nimi&gt;dhl.getSubdivisionList.v2&lt;/xtee:nimi&gt;

&lt;xtee:id&gt;dhl\_client\_1&lt;/xtee:id&gt;

&lt;xtee:toimik/&gt;

&lt;xtee:isikukood&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;/env:Header&gt;

&lt;soapenv:Body&gt;

&lt;dhl:getSubdivisionListResponse&gt;

&lt;paring&gt;

&lt;asutused&gt;142E7683AA18F590EFEC96751000D91F&lt;/asutused&gt;

&lt;/paring&gt;

&lt;keha&gt;

&lt;allyksused href="cid:B6EBE21AD4CC96D41942366EFADCFEFF"/&gt;

&lt;/keha&gt;

&lt;/dhl:getSubdivisionListResponse&gt;

&lt;/soapenv:Body&gt;

&lt;/soapenv:Envelope&gt;

------=\_Part\_22\_9158501.1265809289375

Content-Type: {http://www.w3.org/2001/XMLSchema}base64Binary

Content-Transfer-Encoding: binary

Content-Id: &lt;B6EBE21AD4CC96D41942366EFADCFEFF&gt;

Content-Encoding: gzip

H4sIAAAAAAAAALXSzQqDMAwA4FfxDWTqfg5B2GGHocJwY1cJNGyltZXVDnz7FcWyyUAQPCVNmvAVCihlJ4w...

Y6n+7x3eNW8N/z64R9dnckT7gIAAA==

------=\_Part\_22\_9158501.1265809289375--

##### Päringu „keha“ elemendi sisu

Elemendi „keha“ sisu kodeerimata kujul on:

*&lt;asutused&gt;*

*&lt;asutus&gt;87654321&lt;/asutus&gt;*

*&lt;asutus&gt;12345678&lt;/asutus&gt;*

*&lt;/asutused&gt;*

##### Vastuse „keha“ elemendi sisu

Elemendi „keha“ sisu kodeerimata kujul on:

*&lt;allyksused&gt;*

*&lt;allyksus&gt;*

*&lt;kood&gt;1&lt;/kood&gt;*

*&lt;nimetus&gt;IT osakond&lt;/nimetus&gt;*

*&lt;asutuse\_kood&gt;12345678&lt;/asutuse\_kood&gt;*

*&lt;lyhinimetus&gt;IT&lt;/lyhinimetus&gt;*

*&lt;ks\_allyksuse\_lyhinimetus&gt;YLD&lt;/ks\_allyksuse\_lyhinimetus&gt;*

*&lt;/allyksus&gt;*

*&lt;allyksus&gt;*

*&lt;kood&gt;2&lt;/kood&gt;*

*&lt;nimetus&gt;Üldosakond&lt;/nimetus&gt;*

*&lt;asutuse\_kood&gt;12345678&lt;/asutuse\_kood&gt;*

*&lt;lyhinimetus&gt;YLD&lt;/lyhinimetus&gt;*

*&lt;ks\_allyksuse\_lyhinimetus/&gt;*

*&lt;/allyksus&gt;*

*&lt;allyksus&gt;*

*&lt;kood&gt;12&lt;/kood&gt;*

*&lt;nimetus&gt;Raamatupidamisosakond&lt;/nimetus&gt;*

*&lt;asutuse\_kood&gt;87654321&lt;/asutuse\_kood&gt;*

*&lt;lyhinimetus&gt;RMTP&lt;/lyhinimetus&gt;*

*&lt;ks\_allyksuse\_lyhinimetus/&gt;*

*&lt;/allyksus&gt;*

*&lt;allyksus&gt;*

*&lt;kood&gt;28&lt;/kood&gt;*

*&lt;nimetus&gt;Haldusosakond&lt;/nimetus&gt;*

*&lt;asutuse\_kood&gt;87654321&lt;/asutuse\_kood&gt;*

*&lt;lyhinimetus&gt;haldus&lt;/lyhinimetus&gt;*

*&lt;ks\_allyksuse\_lyhinimetus/&gt;*

*&lt;/allyksus&gt;*

*&lt;/allyksused&gt;*


Päringu nimi: dhl.getOccupationList.v1

Sisendi keha: stringide massiiv

Väljundi keha: massiiv andmetüübist „ametikoht”:

koodstring

nimetusstring

asutuse\_koodstring

lyhinimetusstring

ks\_allyksuse\_lyhinimetusstring(kõrgemalseisva allüksuse lühinimetus)

Päring tagastab sisendparameetriga etteantud asutuste ametikohtade
nimekirja. Päring on vajalik selleks, et oleks võimalik dokumente
adresseerida asutuse ametikohtadele (et oleks teada, millisele
ametikohale DVK serveris milline unikaalne kood vastab).

Antud päringu puhul esitatakse nii sisend- kui väljundandmed pakkimata
ja kodeerimata kujul.

#### Näide

##### Päring

POST /cgi-bin/consumer\_proxy HTTP/1.0

Content-Type: text/xml; charset=utf-8

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"&gt;

&lt;env:Header&gt;

&lt;xtee:asutus&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:ametnik&gt;12345678901&lt;/xtee:ametnik&gt;

&lt;xtee:nimi&gt;dhl.getOccupationList.v1&lt;/xtee:nimi&gt;

&lt;xtee:id&gt;dhl\_client\_1&lt;/xtee:id&gt;

&lt;xtee:toimik/&gt;

&lt;xtee:isikukood&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;/env:Header&gt;

&lt;env:Body&gt;

&lt;dhl:getOccupationList&gt;

&lt;keha xsi:type="SOAP-ENC:Array"
SOAP-ENC:arrayType="xsd:string\[2\]"&gt;

&lt;asutus&gt;12345678&lt;/asutus&gt;

&lt;asutus&gt;23456789&lt;/asutus&gt;

&lt;/keha&gt;

&lt;/dhl:getOccupationList&gt;

&lt;/env:Body&gt;

&lt;/env:Envelope&gt;

##### Päringu vastus

HTTP/1.1 200 OK

SOAPAction: ""

Content-Type: text/xml

&lt;?xml version="1.0" encoding="utf-8"?&gt;

&lt;soapenv:Envelope
xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:dhl="http://www.riik.ee/schemas/dhl"&gt;

&lt;env:Header&gt;

&lt;xtee:asutus&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:ametnik&gt;12345678901&lt;/xtee:ametnik&gt;

&lt;xtee:nimi&gt;dhl.getOccupationList.v1&lt;/xtee:nimi&gt;

&lt;xtee:id&gt;dhl\_client\_1&lt;/xtee:id&gt;

&lt;xtee:toimik/&gt;

&lt;xtee:isikukood&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;/env:Header&gt;

&lt;soapenv:Body&gt;

&lt;dhl:getOccupationListResponse&gt;

&lt;paring xsi:type="SOAP-ENC:Array"
SOAP-ENC:arrayType="xsd:string\[2\]"&gt;

&lt;asutus&gt;12345678&lt;/asutus&gt;

&lt;asutus&gt;23456789&lt;/asutus&gt;

&lt;/paring&gt;

&lt;keha xsi:type="SOAP-ENC:Array"
SOAP-ENC:arrayType="dhl:ametikoht\[4\]"&gt;

&lt;ametikoht&gt;

&lt;kood&gt;2&lt;/kood&gt;

&lt;nimetus&gt;Peadirektor&lt;/nimetus&gt;

&lt;asutuse\_kood&gt;12345678&lt;/asutuse\_kood&gt;

&lt;lyhinimetus&gt;DIR&lt;/lyhinimetus&gt;

&lt;ks\_allyksuse\_lyhinimetus/&gt;

&lt;/ametikoht&gt;

&lt;ametikoht&gt;

&lt;kood&gt;7&lt;/kood&gt;

&lt;nimetus&gt;Finantsdirektor&lt;/nimetus&gt;

&lt;asutuse\_kood&gt;12345678&lt;/asutuse\_kood&gt;

&lt;lyhinimetus&gt;FIN-DIR&lt;/lyhinimetus&gt;

&lt;ks\_allyksuse\_lyhinimetus/&gt;

&lt;/ametikoht&gt;

&lt;ametikoht&gt;

&lt;kood&gt;46&lt;/kood&gt;

&lt;nimetus&gt;Dokumendihaldusosakonna juhataja&lt;/nimetus&gt;

&lt;asutuse\_kood&gt;23456789&lt;/asutuse\_kood&gt;

&lt;lyhinimetus&gt;DH-JUH&lt;/lyhinimetus&gt;

&lt;ks\_allyksuse\_lyhinimetus&gt;DH&lt;/ks\_allyksuse\_lyhinimetus&gt;

&lt;/ametikoht&gt;

&lt;ametikoht&gt;

&lt;kood&gt;92&lt;/kood&gt;

&lt;nimetus&gt;Raamatupidaja&lt;/nimetus&gt;

&lt;asutuse\_kood&gt;23456789&lt;/asutuse\_kood&gt;

&lt;lyhinimetus&gt;RMTP&lt;/lyhinimetus&gt;

&lt;ks\_allyksuse\_lyhinimetus&gt;RMTP&lt;/ks\_allyksuse\_lyhinimetus&gt;

&lt;/ametikoht&gt;

&lt;/keha&gt;

&lt;/dhl:getOccupationListResponse&gt;

&lt;/soapenv:Body&gt;

&lt;/soapenv:Envelope&gt;


Päringu getOccupationList versioon v2 eelneb varasematest versioonidest
selle poolest, et päringu ja vastuse andmed asuvad nüüd SOAP sõnumi
manustes (varasemates versioonides asusid andmed SOAP sõnumi kehas).

Päringu nimi: dhl. getOccupationList.v2

Sisendi keha:

asutusedstringide massiiv päringu MIME manuses base64 kujul

Väljundi keha:

ametikohadmassiiv andmetüübist allyksus päringu MIME manuses base64
kujul

koodstring

nimetusstring

asutuse\_koodstring

lyhinimetusstring

ks\_allyksuse\_lyhinimetusstring(kõrgemalseisva allüksuse lühinimetus)

Päring tagastab sisendparameetriga etteantud asutuste ametikohtade
nimekirja. Päring on vajalik selleks, et oleks võimalik dokumente
adresseerida asutuse ametikohtadele (et oleks teada, millisele
ametikohale DVK serveris milline unikaalne kood vastab).

Antud päringu sisend- ja väljundandmed saadetakse SOAP sõnumi MIME
manustes. Enne SOAP sõnumi manusesse paigutamist pakitakse XML gzip-ga
kokku ja kodeeritakse base64 kujule.

#### Näide

##### Päring

POST /cgi-bin/consumer\_proxy HTTP/1.0

Content-Type: text/xml; charset=utf-8

&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;env:Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:dhl="http://producers.dhl.xtee.riik.ee/producer/dhl"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"&gt;

&lt;env:Header&gt;

&lt;xtee:asutus&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:ametnik&gt;12345678901&lt;/xtee:ametnik&gt;

&lt;xtee:nimi&gt;dhl.getOccupationList.v2&lt;/xtee:nimi&gt;

&lt;xtee:id&gt;dhl\_client\_1&lt;/xtee:id&gt;

&lt;xtee:toimik/&gt;

&lt;xtee:isikukood&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;/env:Header&gt;

&lt;env:Body&gt;

&lt;dhl:getOccupationList&gt;

&lt;keha&gt;

&lt;asutused href="cid:1265809289390"/&gt;

&lt;/keha&gt;

&lt;/dhl:getOccupationList&gt;

&lt;/env:Body&gt;

&lt;/env:Envelope&gt;

------=\_Part\_2\_11235685.1265809289406

Content-Type: {http://www.w3.org/2001/XMLSchema}base64Binary

Content-Transfer-Encoding: binary

Content-Id: &lt;1265809289390&gt;

Content-Encoding: gzip

H4sIAAAAAAAAALNJLC4tKS1OTbGzgbDsLMzNTE2MjQxt9KECMAlDI2MTUzNzC4SEPlwzAG7f6Q9HAAAA

------=\_Part\_2\_11235685.1265809289406—

##### Päringu vastus

HTTP/1.1 200 OK

SOAPAction: ""

Content-Type: text/xml

&lt;?xml version="1.0" encoding="utf-8"?&gt;

&lt;soapenv:Envelope
xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"

xmlns:xsd="http://www.w3.org/2001/XMLSchema"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xmlns:xtee="http://x-tee.riik.ee/xsd/xtee.xsd"

xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"

xmlns:dhl="http://www.riik.ee/schemas/dhl"&gt;

&lt;env:Header&gt;

&lt;xtee:asutus&gt;12345678&lt;/xtee:asutus&gt;

&lt;xtee:andmekogu&gt;dhl&lt;/xtee:andmekogu&gt;

&lt;xtee:ametnik&gt;12345678901&lt;/xtee:ametnik&gt;

&lt;xtee:nimi&gt;dhl.getOccupationList.v2&lt;/xtee:nimi&gt;

&lt;xtee:id&gt;dhl\_client\_1&lt;/xtee:id&gt;

&lt;xtee:toimik/&gt;

&lt;xtee:isikukood&gt;EE12345678901&lt;/xtee:isikukood&gt;

&lt;/env:Header&gt;

&lt;soapenv:Body&gt;

&lt;dhl:getOccupationListResponse&gt;

&lt;paring&gt;

&lt;asutused&gt;142E7683AA18F590EFEC96751000D91F&lt;/asutused&gt;

&lt;/paring&gt;

&lt;keha&gt;

*&lt;ametikohad href="cid:* *DD284357A5C7762760F2BA30D7AD3B48"/&gt;*

&lt;/keha&gt;

&lt;/dhl:getOccupationListResponse&gt;

&lt;/soapenv:Body&gt;

&lt;/soapenv:Envelope&gt;

------=\_Part\_22\_9158501.1265809289375

Content-Type: {http://www.w3.org/2001/XMLSchema}base64Binary

Content-Transfer-Encoding: binary

*Content-Id: &lt;* *DD284357A5C7762760F2BA30D7AD3B48&gt;*

Content-Encoding: gzip

H4sIAAAAAAAAALNJzE0tyczOz0hMsbOBsUvsbLLz81PsDG30wbRNXiZQprTYLvjwnpyUTIWy/KLczKxEG32...

Y4VR/JCwDpUv8hzwAAAA==

------=\_Part\_22\_9158501.1265809289375--

##### Päringu „keha“ elemendi sisu

Elemendi „keha“ sisu kodeerimata kujul on:

*&lt;asutused&gt;*

*&lt;asutus&gt;87654321&lt;/asutus&gt;*

*&lt;asutus&gt;12345678&lt;/asutus&gt;*

*&lt;/asutused&gt;*

##### Vastuse „keha“ elemendi sisu

Elemendi „keha“ sisu kodeerimata kujul on:

*&lt;ametikohad&gt;*

*&lt;ametikoht&gt;*

*&lt;kood&gt;2&lt;/kood&gt;*

*&lt;nimetus&gt;Peadirektor&lt;/nimetus&gt;*

*&lt;asutuse\_kood&gt;12345678&lt;/asutuse\_kood&gt;*

*&lt;lyhinimetus&gt;DIR&lt;/lyhinimetus&gt;*

*&lt;ks\_allyksuse\_lyhinimetus/&gt;*

*&lt;/ametikoht&gt;*

*&lt;ametikoht&gt;*

*&lt;kood&gt;7&lt;/kood&gt;*

*&lt;nimetus&gt;Finantsdirektor&lt;/nimetus&gt;*

*&lt;asutuse\_kood&gt;12345678&lt;/asutuse\_kood&gt;*

*&lt;lyhinimetus&gt;FIN-DIR&lt;/lyhinimetus&gt;*

*&lt;ks\_allyksuse\_lyhinimetus/&gt;*

*&lt;/ametikoht&gt;*

*&lt;ametikoht&gt;*

*&lt;kood&gt;46&lt;/kood&gt;*

*&lt;nimetus&gt;Dokumendihaldusosakonna juhataja&lt;/nimetus&gt;*

*&lt;asutuse\_kood&gt;87654321&lt;/asutuse\_kood&gt;*

*&lt;lyhinimetus&gt;DH-JUH&lt;/lyhinimetus&gt;*

*&lt;ks\_allyksuse\_lyhinimetus&gt;DH&lt;/ks\_allyksuse\_lyhinimetus&gt;*

*&lt;/ametikoht&gt;*

*&lt;ametikoht&gt;*

*&lt;kood&gt;92&lt;/kood&gt;*

*&lt;nimetus&gt;Raamatupidaja&lt;/nimetus&gt;*

*&lt;asutuse\_kood&gt;87654321&lt;/asutuse\_kood&gt;*

*&lt;lyhinimetus&gt;RMTP&lt;/lyhinimetus&gt;*

*&lt;ks\_allyksuse\_lyhinimetus&gt;RMTP&lt;/ks\_allyksuse\_lyhinimetus&gt;*

*&lt;/ametikoht&gt;*

*&lt;/ametikohad&gt;*


Dokumendivahetuskeskuses kasutatakse kahetasemelist kasutusõiguste
süsteemi:

-   X-Tee vahenditega autentimine ja autoriseerimine
-   DVK kasutajate andmebaasi põhjal autentimine ja autoriseerimine

DVK serveri kasutamiseks peavad seega esmajoones olema RIA turvaserveris
asutusele avatud DVK päringud. Vastasel juhul ei jõua DVK serverile
saadetud päringud DVK serverisse kohale.

Kui DVK päringud on RIA turvaserveris lubatud, siis jõuavad päringud DVK
serverisse ja läbivad seal järgmise kontrolli:

-   **Kas päringu saatnud asutus on registreeritud DVK asutuste
    registris?**\
    Kui asutus ei ole DVK asutuste registris registreeritud, siis
    päringu töötlemine katkestatakse, kuna päringu töötlemiseks (näit.
    adresseerimiseks) vajalikke andmeid ei ole olemas.

<!-- -->

-   **Kas päringu saatnud isik on registreeritud DVK isikute
    registris?**\
    Kui päringu saatja ei ole isikute registris registreeritud, siis
    päringu töötlemine katkestatakse, kuna pole võimalik määrata
    konkreetse isiku kasutajaõigusi.

Sõltuvalt tehtavast päringust kontrollib DVK server veel järgmisi
tingimusi:

-   sendDocuments

<!-- -->

-   -   Saadetava dokumendi adressaadiks olev asutus peab olema
        registreeritud DVK asutuste registrisse.

<!-- -->

-   receiveDocuments

<!-- -->

-   -   Kui päringu teinud isik täidab oma asutuses ametikohta, mille
        rolliks on „DHL: Asutuse administraator“, siis saab ta alla
        laadida kõik antud asutusele adresseeritud dokumendid.
    -   Kui päringu teinud isik töötab oma asutuses mingis allüksuses,
        siis saab ta alla laadida kõik antud allüksusele
        adresseeritud dokumendid.
    -   Kui päringu teinud isik täidab oma asutuses mingit ametikohta,
        siis saab ta alla laadida kõik antud ametikohale
        adresseeritud dokumendid.
    -   Päringu teinud isik saab alla laadida kõik isiklikult talle
        adresseeritud dokumendid.

<!-- -->

-   getSendStatus

<!-- -->

-   -   Dokumendi staatust saab pärida ainult nende dokumentide kohta,
        mille on saatnud antud päringu teinud asutus (s.t. teiste
        asutuste poolt saadetud dokumentide kohta tagasisidet küsida
        ei saa).

Üldjuhul, kus DVK andmevahetuse eest hoolitseb tarkvara ja reaalsed
isikud otseselt DVK-le päringuid ei esita, on DVK andmevahetuse
korrektseks toimimiseks seega vaja, et isik, kelle nimel DVK-le
päringuid esitatakse, täidaks DVK ametikohtade registris ametikohta,
mille rolliks on määratud „DHL: Asutuse administraator“.


Päringuga sendDocuments dokumentide saatmisel toimub DVK serveris
dokumentide valideerimine. DVK server teostab järgmised kontrollid:

1.  DVK konteineri XML struktuuri korrektse vorminduse
    *(well formed)*kontroll
2.  DVK konteineri XML struktuuri vastavus konteineri XML skeemile
    (teostatakse juhul, kui DVK konteineri XML-s on viidatud konteineri
    XML skeemile)
3.  DVK konteineri &lt;metaxml&gt; elemendis sisalduva XML-i struktuuri
    vastavus XML skeemile (teostatakse juhul, kui &lt;metaxml&gt;
    elemendis on viidatud vastavale XML skeemile.
4.  &lt;SignedDoc&gt; või &lt;Failid&gt; elemendis edastatavate
    digiallkirja konteinerite (.DDOC või .BDOC failid) allkirjade
    kehtivuse kontroll.
5.  &lt;SignedDoc&gt; või &lt;Failid&gt; elemendis edastatava XML faili
    korrektse vorminduse *(well formed)*kontroll.
6.  &lt;SignedDoc&gt; või &lt;Failid&gt; elemendis edastatava XML faili
    struktuuri vastavus vastavale XML skeemile (teostatakse juhul, kui
    XML failis on viidatud vastavale XML skeemile).
7.  &lt;SignedDoc&gt; või &lt;Failid&gt; elemendis edastatava
    digiallkirja konteinerite (.DDOC või .BDOC failid) sisus asuvate XML
    failide korrektse vorminduse *(well formed)*kontroll.
8.  &lt;SignedDoc&gt; või &lt;Failid&gt; elemendis edastatava
    digiallkirja konteinerite (.DDOC või .BDOC failid) sisus asuvate XML
    failide vastavus vastavale XML skeemile (teostatakse juhul, kui XML
    failis on viidatud vastavale XML skeemile).

Valideerimisel avastatud vea puhul tagastab sendDocuments päring
dokumendi saatjale veateate vea võimalikult detailse kirjeldusega.
Veateade tagastatakse päringu käivitajale SOAP Fault sõnumina. Vea
kirjeldus sisaldab sõltuvalt teostatud kontrollist järgmisi andmeid:

-   XML failide valideerimisel vea kirjeldust, XML faili nime ja vea
    asukohta failis (rida, veerg)
-   Digitaalallkirjade puhul vea kirjeldust ja faili nime.

Nii XML failide kui ka digitaalallkirjade valideerimist saab DVK serveri
konfiguratsioonis vajadusel välja lülitada. Selleks tuleb serveri
konfiguratsioonifailis dhl.properties muuta parameetrite
„server\_validate\_container“, „server\_validate\_xml“ ja
„server\_validate\_signatures“ väärtusi.

***Faili dhl.properties näidis:**\
*

\# Konteineri valideerimise seaded

*server\_validate\_container = no*

*server\_ignore\_invalid\_containers = no*

\# Määrab, kas DVK server üritab valideerida konteineri sisus

\# edastatavaid XML faile.

*server\_validate\_xml\_files = no*

\# Määrab, kas DVK server üritab valideerida digiallkirja

\# konteinerite küljes olevaid allkirju.

*server\_validate\_signatures = no*

**


DVK serverit on võimalik seadistada nii, et kui saadetav
dokumendikonteiner vastab etteantud tingimustele, siis lisatakse
dokumendi adressaatide hulka üks või mitu täiendavat adressaati.
Nimetatud lahendus on vajalik näiteks selleks, et garanteerida mingi
projektiga seotud dokumentide jõudmine kõigile asjassepuutuvatele
osapooltele.

Automaatse adressaatide lisamise korral muudab DVK server saadetava
dokumendikonteineri XML andmeid, s.t. lisatud adressaadid on nähtavad ka
kõigile teistele adressaatidele ja dokumendi esialgsele saatjale.


DVK serveri poolt automaatselt lisatavaid aadressaate saab seadistada
DVK serveri andmetabelis VASTUVOTJA\_MALL. Nimetatud andmetabeli
struktuur näeb välja järgmine:

XPATH tingimuse näiteid:

1.  Oletame, et adressaat tuleks lisada dokumendile, mille adressaatide
    hulgas on juba olemas asutus registrikoodiga 12345678. Sellisel
    juhul näeks andmevälja TINGIMUS\_XPATH väärtus välja järgmine:\
    */dokument/transport/saaja\[regnr='12345678'\]*
2.  Oletame, et adressaat tuleks lisada dokumendile, mille saatjaks on
    asutus registrikoodiga 23456789. Sellisel juhul näeks andmevälja
    TINGIMUS\_XPATH väärtus välja järgmine:\
    */dokument/transport/saatja\[regnr='23456789'\]*
3.  Oletame, et adressaat tuleks lisada dokumendile, mille metainfo
    blokis on saatja defineeritud andmeväli „Lisa mulle aadress“ ja
    väärtusega „Jah“. Sellisel juhul näeks andmeväli TINGIMUS\_XPATH
    väärtus välja järgmine:\
    */dokument/metainfo\[saatja\_defineeritud='Jah'\]/saatja\_defineeritud\[@valjanimi='Lisa
    mulle aadress'\]*
4.  Oletame, et adressaat tuleks lisada dokumendile, mille metainfo
    blokis on saatja defineeritud andmeväli „Projekt“ ja
    väärtusega „DVK“. Sellisel juhul näeks andmeväli TINGIMUS\_XPATH
    väärtus välja järgmine:\
    */dokument/metainfo\[saatja\_defineeritud='DVK'\]/saatja\_defineeritud\[@valjanimi='Projekt'\]*
5.  Oletame, et adressaat tuleks lisada dokumendile, mille metaxml blokk
    sisaldab järgmises XML näites paksemas kirjas esitatud väärtust:

*&lt;dokument&gt;\
...\
&lt;metaxml&gt;\
&lt;lepingu\_andmed&gt;\
&lt;osapooled&gt;\
&lt;osapool&gt;\
&lt;registrikood&gt;**12345678**&lt;/registrikood&gt;\
&lt;nimetus&gt;Ettevote1 AS&lt;/nimetus&gt;\
&lt;/osapool&gt;\
&lt;/osapooled&gt;\
&lt;/lepingu\_andmed&gt;\
&lt;/metaxml&gt;\
...\
&lt;/dokument&gt;*

Sellisel juhul näeks andmeväli TINGIMUS\_XPATH väärtus välja järgmine:\
*/dokument/metaxml/lepingu\_andmed/osapooled/osapool\[registrikood='12345678'\]*


DVK lüüsid kujutavad endast võimalust edastada DVK serverisse saadetud
dokumente mõnda teise DVK serverisse või mõnda teise
dokumendivahetussüsteemi. Esmases tehnilises lahenduses toetab DVK
dokumentide edastamist DVK andmevahetusspetsifikatsioonile vastavatesse
dokumendivahetussüsteemidesse.

Sellise dokumentide edastamise peamiseks eesmärgiks on, et saaks
eksisteerida eraldi dokumendivahetuskeskkonnad näiteks riigisektori ja
erasektori jaoks. Dokumentide vahetamine kirjeldatud juhul toimiks siis
joonisel 1 toodud skeemi alusel:

Joonis 1

DVK serverisse saadetud dokumendi (päring *sendDocuments*) edastamise
protsess on esitatud joonisel 2.

Joonis 2

Analoogilist protsessi rakendatakse ka olukorras, kus dokumendi saatja
pärib andmeid dokumendi staatuse kohta (päring *getSendStatus*).


DVK arhitektuurist tingitult on DVK lüüsidele seatud järgmised
tehnilised piirangud:

1.  Dokumendivahetussüsteem, kuhu DVK server dokumente edastab, peab
    toetama DVK andmevahetusspetsifikatsioonile
    sarnast transaktsiooniloogikat. S.t. DVK-ga liidestatav
    dokumendivahetussüsteem peab suutma anda ja vastu võtta andmeid
    dokumendi kohaletoimetamise kohta.\
    Vastasel juhul puudub DVK kaudu dokumendi välja saatnud asutusel või
    isikul võimalus teada saada, kas tema poolt saadetud dokument on
    edukalt kohale toimetatud.
2.  Dokumendivahetussüsteem, kuhu DVK server dokumente edastab, peab
    toetama DVK dokumendikonteineri spetsifikatsioonile vastavate XML
    andmevahetuskonteinerite kasutamist. Alternatiivina võib liidestatav
    dokumendivahetussüsteem kasutada andmevahetuskonteinerit, mis on
    andmevahetuse toimimise seisukohast kriitiliste andmete osas
    teisendatav DVK andmevahetuskonteineriks (ja vastupidi).
3.  Iga DVK server peab omama kõigi teiste liidestatud
    dokumendivahetusserverite nimekirja ning omama ligipääsu nendes
    serverites seadistatud asutuste nimekirjale.\
    Kui eeldada, et iga DVK server ei ole teadlik kõigist teistest DVK
    serveritest, siis tuleks dokumendi edastamisel arvestada vajadusega
    edastada dokument adressaadile läbi mitme serveri. Iga
    serveritevaheline edastus tähendaks aga saatmisele kuluva aja
    täiendavat kasvu (dokumendi edastamine läbi 10 serveri oleks kõigi
    serverite vahel sama andmesidekiirust eeldades ca. 11 korda aeglasem
    kui otse saatmine).\
    Kui eeldada, et iga DVK server ei oma ligipääsu võimalike
    adressaatide nimekirjale, siis ei ole võimalik dokumente edastada.
4.  Iga DVK server, mis on võimeline dokumente edastama, peab omama
    asutuse registrikoodi ja isikukoodi, mida kasutades dokumente edasi
    saadetakse.\
    Vastasel juhul ei ole võimalik DVK serverist andmeid üle X-Tee

Tehniline lahendus jääb dokumendi saatja seisukohast täpselt
samasuguseks nagu varem. S.t. saatja koostab saadetavatest dokumentidest
DVK konteineri, lisab enda andmed ja adressaatide andmed ning saadab
konteineri oma DVK serverisse. Dokumendi kohaletoimetamine on sellest
hetkest alates DVK serverite omavaheline asi.


Vastuvõtja seisukohast lisandub käesoleva lahendusega täiendav kirje
„vahendaja“ DVK konteineri transport plokis. Antud kirje näol on
tegemist automaatselt täidetavate andmetega, mille lisab DVK
dokumendikonteinerisse dokumendi edastanud DVK server.

*&lt;dhl:transport&gt;*

*&lt;dhl:saatja&gt;*

*&lt;dhl:regnr&gt;11111111&lt;/dhl:regnr&gt;*

*&lt;dhl:asutuse\_nimi&gt;Asutuse nimi&lt;/dhl:asutuse\_nimi&gt;*

*&lt;dhl:allyksuse\_kood&gt;10&lt;/dhl:allyksuse\_kood&gt;*

*&lt;dhl:allyksuse\_nimetus&gt;Saatmisosakond&lt;/dhl:allyksuse\_nimetus&gt;*

*&lt;dhl:ametikoha\_kood&gt;20&lt;/dhl:ametikoha\_kood&gt;*

*&lt;dhl:ametikoha\_nimetus&gt;Saatja&lt;/dhl:ametikoha\_nimetus&gt;*

*&lt;dhl:isikukood&gt;30000000001&lt;/dhl:isikukood&gt;*

*&lt;dhl:nimi&gt;Isiku nimi&lt;/dhl:nimi&gt;*

*&lt;dhl:epost&gt;epost@saatja.ee&lt;/dhl:epost&gt;*

*&lt;dhl:osakonna\_kood&gt;Osakonna kood&lt;/dhl:osakonna\_kood&gt;*

*&lt;dhl:osakonna\_nimi&gt;Osakonna nimi&lt;/dhl:osakonna\_nimi&gt;*

*&lt;/dhl:saatja&gt;*

*&lt;dhl:saaja&gt;*

*&lt;dhl:regnr&gt;33333333&lt;/dhl:regnr&gt;*

*&lt;dhl:asutuse\_nimi&gt;Asutuse nimi&lt;/dhl:asutuse\_nimi&gt;*

*&lt;dhl:allyksuse\_kood&gt;11&lt;/dhl:allyksuse\_kood&gt;*

*&lt;dhl:allyksuse\_nimetus&gt;Vastuvõtuosakond&lt;/dhl:allyksuse\_nimetus&gt;*

*&lt;dhl:ametikoha\_kood&gt;21&lt;/dhl:ametikoha\_kood&gt;*

*&lt;dhl:ametikoha\_nimetus&gt;Vastuvõtja&lt;/dhl:ametikoha\_nimetus&gt;*

*&lt;dhl:isikukood&gt;31111111112&lt;/dhl:isikukood&gt;*

*&lt;dhl:nimi&gt;Isiku nimi&lt;/dhl:nimi&gt;*

*&lt;dhl:epost&gt;epost@vastuvotja.ee&lt;/dhl:epost&gt;*

*&lt;dhl:osakonna\_kood&gt;Osakonna kood&lt;/dhl:osakonna\_kood&gt;*

*&lt;dhl:osakonna\_nimi&gt;Osakonna nimi&lt;/dhl:osakonna\_nimi&gt;*

*&lt;/dhl:saaja&gt;*

***** **&lt;dhl:vahendaja&gt;***

***** **&lt;dhl:regnr&gt;22222222&lt;/dhl:regnr&gt;***

***&lt;dhl:asutuse\_nimi&gt;Asutuse nimi&lt;/dhl:asutuse\_nimi&gt;***

&lt;dhl:allyksuse\_kood/&gt;

***&lt;dhl:allyksuse\_nimetus/&gt;***

&lt;dhl:ametikoha\_kood/&gt;

***&lt;dhl:ametikoha\_nimetus/&gt;***

***** **&lt;dhl:isikukood&gt;40000000001&lt;/dhl:isikukood&gt;***

***** **&lt;dhl:nimi&gt;Isiku nimi&lt;/dhl:nimi&gt;***

***** **&lt;dhl:epost&gt;epost@vahendaja.ee&lt;/dhl:epost&gt;***

***** **&lt;dhl:osakonna\_kood/&gt;***

***** **&lt;dhl:osakonna\_nimi/&gt;***

***** **&lt;/dhl:vahendaja&gt;***

*&lt;/dhl:transport&gt;*

Vahendaja kirje on ennekõike vajalik selleks, et DVK server lubaks
dokumendiedastuse vahendajaks olnud asutusel teostada dokumendi edastuse
staatust puudutavaid päringuid (üldjuhul on see õigus üksnes dokumendi
saatjal).

Teine oluline põhjus vahendaja kirje lisamiseks on asjaolu, et vastasel
juhul peaks DVK server olema valmis vastu võtma dokumente, mille saatja
andmed ei klapi X-tee päringu teinud asutuse andmetega. See aga annaks
võimaluse tahtmatuteks (või ka tahtlikeks) identiteedivargusteks, mille
lahendamine oleks võimalik üksnes X-Tee logide abil.


Et DVK server saaks teistesse serveritesse dokumente edastada, peavad
olema täidetud järgmised tingimused:

1.  Server peab saama teostada X-Tee päringuid
2.  Server peab teadma teiste dokumendivahetussüsteemide serverite
    aadresse (s.t. peab teadma, kuhu dokumente saata saab)
3.  Asutuse turvaserver peab lubama DVK päringuid teistest DVK
    serveritest ja teiste DVK serverite turvaserverid peavad lubama
    päringuid antud asutuse turvaserverist.

#### <span id="anchor-813"></span>DVK serveri seadistamine X-Tee päringute teostamiseks

Kuna dokumentide edastamine ühest DVK serverist teise toimub X-Tee
päringutega, siis peab DVK serveril olema seadistatud asutuse
registrikood ja isikukood, mille nimel X-Tee päringuid teostatakse.

Selleks tuleb DVK serveri konfiguratsioonifaili dhl.properties lisada
järgmised read:

*client\_default\_org\_code = 12345678*

*client\_default\_person\_code = 11111111111*

#### <span id="anchor-814"></span>Väliste serverite aadresside lisamine DVK serverisse

Et DVK server teaks, millised teised DVK serverid olemas on ja kus need
asuvad, tuleb DVK serverile ette anda teadaolevate teiste DVK serverite
nimekiri.

Selleks tuleks iga teadaoleva teise DVK serveri kohta lisada
andmetabelisse „Server“ järgmised andmed:

-   andmekogu nimetusnäiteks „dhl“. Ei pea olema täidetud, kui server ei
    kasuta andmevahetuseks X-Teed.
-   aadressX-Tee andmevahetuse puhul reeglina:\
    http://\[TURVASERVER\]/cgi-bin/consumer\_proxy\
    Ilma X-Tee vahenduseta andmevahetuse puhul oleks siin serveri
    reaalne URL.


DVK päringuid sendDocuments.v2 ja receiveDocuments.v2 saab kasutada nii,
et dokumendid edastataks kliendilt serverile või serverilt kliendile
tükkhaaval.

Dokumentide tükkhaaval saatmine ja vastuvõtmine on kasulik ennekõike
suuremahuliste dokumentide puhul, kui:

-   suuremahulise dokumendi saatmine või vastuvõtmine võtab kaua aega,
    mistõttu DVK server, infosüsteem või mõni vahepealne X-Tee
    turvaserver võib timeout’i tõttu andmete edastamise katkestada.
-   suuremahulise dokumendi jaotamine väiksemateks tükkideks aitab
    vähendada DVK serveri või klientrakenduse töökoormust.

Dokumentide tükkhaaval saatmisel on eeldatud, et saadetavad andmed on
esmalt GZIP-iga kokku pakitud ja alles seejärel tükkideks jaotatud.
Analoogilist loogikat on rakendatud ka dokumentide vastuvõtmisel, s.t.
DVK server pakib andmed esmalt kokku ning jaotab alles seejärel
tükkideks.

Dokumentide tükkhaaval saatmisel tuleks kasutada järgmisi päringu
sendDocuments.v2 parameetreid:

-   fragmente\_kokkuEdastatavate fragmentide hulk (s.t. mitmeks tükiks
    dokument on jagatud)
-   fragment\_nrEdastatava fragmendi järjekorranumber alates 0-st (s.t.
    0, 1, 2, jne.)
-   edastus\_idEdastussessiooni ID. Saatja poolt vabalt valitav
    võimalikult unikaalne string, mis on ühiseks nimetajaks kõigile
    edastatavatele tükkidele.

Dokumentide tükkhaaval vastuvõtmiseks tuleks kasutada järgmisi päringu
receiveDocuments.v2 parameetreid:

-   fragmendi\_suurus\_baitidesFragmendi soovitav suurus baitides. Annab
    DVK serverile teada, kui suurteks tükkideks see peaks kliendile
    saadetavad andmed jaotama.
-   fragment\_nrJärgmise oodatava fragmendi järjekorranumber alates
    0-st (s.t. 0, 1, 2, jne.)
-   edastus\_idEdastussessiooni ID. Vastuvõtja poolt vabalt valitav
    võimalikult unikaalne string, mis on ühiseks nimetajaks kõigile
    edastatavatele tükkidele (ja mille alusel saab hiljem kõik tükid
    tuvastada ja kokku panna).


DVK rakendus sisaldab kasutatavatest tarkvarakomponentidest (Axis 1.3
teegist) tingituna järgmist viga MIME sõnumimanuste
Content-Transfer-Encoding päises:

Et MIME manustena saadetakse Base64 kodeeritud binaarfaile, siis peaks
manused saama endale järgmise päise:

Content-Type:{http://www.w3.org/2001/XMLSchema}base64Binary

*Content-Transfer-Encoding:**base64***

Content-Encoding: gzip

Content-ID: ...

DVK rakendus väljastab aga kõik MIME manused järgmise päisega:

Content-Type:{http://www.w3.org/2001/XMLSchema}base64Binary

*Content-Transfer-Encoding:**binary***

Content-Encoding: gzip

*Content-ID: ...*

Antud juhul tuleks arvestada, et hoolimata päises märgitud *binary*
kodeeringust saadab DVK MIME manuseid ikkagi Base64 kodeeritult. Samuti
ignoreerib DVK rakendus antud päist saabuvate sõnumite puhul ning
eeldab, et manus on saadetud Base64 kodeeritud kujul.


DVK rakendus ei suuda päringut korrektselt vastu võtta, kui saadetava
sõnumi HTTP päises puuduvad jutumärgid *Content-Type* päises parameetri
*type* väärtuse ümber. Puuduvate jutumärkide korral ei suuda DVK
rakendus sõnumit töödelda ning tagastab veateate.

DVK päringud töötavad korrektselt näiteks järgmise päise korral:

POST /cgi-bin/consumer\_proxy HTTP/1.0

*Content-Type: multipart/related; **type="text/xml";**
start="&lt;FC392E97EB481BFCEB435125AA3B5B51&gt;";
boundary="----=\_Part\_0\_20230270.1171971312715"*

User-Agent: Axis/1.3

...

DVK päringud ei tööta aga korrektselt näiteks järgmise päise korral:

POST /cgi-bin/consumer\_proxy HTTP/1.0

*Content-Type: multipart/related; **type=text/xml**;
start="&lt;FC392E97EB481BFCEB435125AA3B5B51&gt;";
boundary="----=\_Part\_0\_20230270.1171971312715"*

User-Agent: Axis/1.3

...

Antud viga põhjustab Apache Axis 1.3 koosseisus kasutatav JavaMail teek,
mis eeldab nimetatud jutumärkide olemasolu.


Alates versioonist 1.6.0 on kasutusel uus versioon DVK konteinerist.
Seoses uue versiooni kasutuselevõtuga tekkisid ka uued nimeruumid
manuaalsete metaandmete ja dokumenti kirjeldavate elementide jaoks. DVK
konteineri uus versioon (2) töötab paralleelselt vanema versiooniga (1).
Olenevalt päringu versioonist on kasutusel kas DVK konteineri versioon 1
või 2.


Automaatsete metaandmete koosseis ei muutunud seoses DVK konteineri
versiooni 2 kasutuselevõtuga.

Nimeruumi väärtuseks on: *http://www.riik.ee/schemas/dhl-meta-automatic*

XML skeemifaili asukoht on:
*http://www.riik.ee/schemas/dhl/dhl-meta-automatic.xsd*


Manuaalsete metaandmete koosseis muutus seoses DVK konteineri versiooni
2 kasutuselevõtuga:

DVK konteineri versioon 1 puhul:

-   Nimeruumi väärtuseks on:
    *http://www.riik.ee/schemas/dhl-meta-manual*
-   XML skeemifaili asukoht on:
    http://www.riik.ee/schemas/dhl/dhl-meta-manual.xsd

DVK konteineri versioon 2 puhul:

-   Nimeruumi väärtuseks on:
    *http://www.riik.ee/schemas/dhl-meta-manual/2010/r1*
-   XML skeemifaili asukoht on:
    *http://www.riik.ee/schemas/dhl/dhl-meta-manual.2010.r1.xsd*


Dokumendi andmete koosseis muutus seoses DVK konteineri versiooni 2
kasutuselevõtuga. Toimivad järgmised nimeruumid:

DVK konteineri versioon 1 puhul:

-   Nimeruumi väärtuseks on: *http://www.riik.ee/schemas/dhl*

<!-- -->

-   *XML skeemifaili asukoht on: http://www.riik.ee/schemas/dhl/dhl.xsd*

DVK konteineri versioon 2 puhul:

-   Nimeruumi väärtuseks on: *http://www.riik.ee/schemas/dhl/2010/r1*

<!-- -->

-   XML skeemifaili asukoht on:
    *http://www.riik.ee/schemas/dhl/dhl.2010.r1.xsd*


Päringute WSDL kirjeldus asub failis dhl.wsdl. See fail asub DVK serveri
paketis juurkaustas. SVN-is:
<https://svn.eesti.ee/projektid/dvk/server/trunk/src/main/webapp/dhl.wsdl><span
id="anchor-929"></span>Päringute WSDL kirjeldus asub failis dhl.wsdl.
See fail asub DVK serveri paketis juurkaustas. SVN-is:
<https://svn.eesti.ee/projektid/dvk/server/trunk/src/main/webapp/dhl.wsdl>

<span id="anchor-930"></span>****LISA 2: &lt;dokument&gt; XML struktuuri kasutusnäide (DVK konteineri versioon 1)****
=====================================================================================================================

*&lt;dhl:dokument xmlns:dhl="http://www.riik.ee/schemas/dhl"
dhl:schemaLocation=““&gt;*

*&lt;dhl:metainfo
xmlns:ma="http://www.riik.ee/schemas/dhl-meta-automatic"
ma:schemaLocation=“*
*http://www.riik.ee/schemas/dhl/dhl-meta-automatic.xsd“*

*xmlns:mm="http://www.riik.ee/schemas/dhl-meta-manual"
mm:schemaLocation=“*
*http://www.riik.ee/schemas/dhl/dhl-meta-manual.xsd“&gt;*

*&lt;!— Seda osa pole ise vaja täita, selle täidab DVK väljuval sõnumil
ise --&gt;*

*&lt;ma:dhl\_id&gt;100&lt;/ma:dhl\_id&gt;*

*&lt;ma:dhl\_saabumisviis&gt;xtee&lt;/ma:dhl\_saabumisviis&gt;*

*&lt;ma:dhl\_saabumisaeg&gt;2006-03-16T14:11:23+02:00&lt;/ma:dhl\_saabumisaeg&gt;*

*&lt;ma:dhl\_saatmisviis&gt;xtee&lt;/ma:dhl\_saatmisviis&gt;*

*&lt;ma:dhl\_saatmisaeg&gt;2006-03-15T18:17:00+02:00&lt;/ma:dhl\_saatmisaeg&gt;*

*&lt;ma:dhl\_saatja\_asutuse\_nr&gt;12345678&lt;/ma:dhl\_saatja\_asutuse\_nr&gt;*

*&lt;ma:dhl\_saatja\_isikukood&gt;37501010001&lt;/ma:dhl\_saatja\_isikukood&gt;*

*&lt;ma:dhl\_saaja\_asutuse\_nr&gt;87654321&lt;/ma:dhl\_saaja\_asutuse\_nr&gt;*

*&lt;ma:dhl\_saaja\_isikukood&gt;37005050005&lt;/ma:dhl\_saaja\_isikukood&gt;*

*&lt;ma:dhl\_saatja\_epost&gt;hunt.kriimsilm@kuivendus.ee&lt;/ma:dhl\_saatja\_epost&gt;*

*&lt;ma:dhl\_saaja\_epost&gt;karupoeg.puhh@vallavalitsus.ee&lt;/ma:dhl\_saaja\_epost&gt;*

*&lt;ma:dhl\_kaust&gt;/KIRJAD&lt;/ma:dhl\_kaust&gt;*

*&lt;!— Automaatselt täidetava osa lõpp --&gt;*

*&lt;mm:koostaja\_asutuse\_nr&gt;12345678&lt;/mm:koostaja\_asutuse\_nr&gt;*

*&lt;mm:saaja\_asutuse\_nr&gt;87654321&lt;/mm:saaja\_asutuse\_nr&gt;*

*&lt;mm:koostaja\_dokumendinimi&gt;Kaeveloa taotlus, H.
Kriimsilm&lt;/mm:koostaja\_dokumendinimi&gt;*

*&lt;mm:koostaja\_dokumendityyp&gt;Taotlus&lt;/mm:koostaja\_dokumendityyp&gt;*

*&lt;mm:koostaja\_dokumendinr&gt;A-101&lt;/mm:koostaja\_dokumendinr&gt;*

*&lt;mm:koostaja\_failinimi&gt;taotlus\_hkriimsilm\_110306.doc&lt;/mm:koostaja\_failinimi&gt;*

*&lt;mm:koostaja\_kataloog&gt;/home/users/kriimsilm/dokumendid&lt;/mm:koostaja\_kataloog&gt;*

*&lt;mm:koostaja\_votmesona&gt;taotlus,
kraav&lt;/mm:koostaja\_votmesona&gt;*

*&lt;mm:koostaja\_kokkuvote&gt;*

*Kaeveloa taotlus 200m kraavi kaevamiseks kahe kinnistu*

piirile N maakonna X vallas.

*&lt;/mm:koostaja\_kokkuvote&gt;*

*&lt;mm:koostaja\_kuupaev&gt;2006-02-11T14:11:23+02:00&lt;/mm:koostaja\_kuupaev&gt;*

*&lt;mm:koostaja\_asutuse\_nimi&gt;Kuivendusekspert
OÜ&lt;/mm:koostaja\_asutuse\_nimi&gt;*

*&lt;mm:koostaja\_asutuse\_kontakt&gt;6 543
210&lt;/mm:koostaja\_asutuse\_kontakt&gt;*

*&lt;mm:autori\_osakond&gt;Projekteerimisosakond&lt;/mm:autori\_osakond&gt;*

*&lt;mm:autori\_isikukood&gt;37501010001&lt;/mm:autori\_isikukood&gt;*

*&lt;mm:autori\_nimi&gt;Hunt Kriimsilm&lt;/mm:autori\_nimi&gt;*

*&lt;mm:autori\_kontakt&gt;56 123 456&lt;/mm:autori\_kontakt&gt;*

*&lt;mm:seotud\_dokumendinr\_koostajal&gt;B-005&lt;/mm:seotud\_dokumendinr\_koostajal&gt;*

*&lt;mm:seotud\_dokumendinr\_saajal&gt;KT-2006-14&lt;/mm:seotud\_dokumendinr\_saajal&gt;*

*&lt;mm:saatja\_dokumendinr&gt;A-101&lt;/mm:saatja\_dokumendinr&gt;*

*&lt;mm:saatja\_kuupaev&gt;2006-02-12T01:02:03+02:00&lt;/mm:saatja\_kuupaev&gt;*

*&lt;mm:saatja\_asutuse\_kontakt&gt;6 543
210&lt;/mm:saatja\_asutuse\_kontakt&gt;*

*&lt;mm:saaja\_isikukood&gt;37005050005&lt;/mm:saaja\_isikukood&gt;*

*&lt;mm:saaja\_nimi&gt;Karupoeg Puhh&lt;/mm:saaja\_nimi&gt;*

*&lt;mm:saaja\_osakond&gt;Maaparandusosakond&lt;/mm:saaja\_osakond&gt;*

*&lt;mm:saatja\_defineeritud
mm:valjanimi="PlaneeringuNr"&gt;1825/1&lt;/mm:saatja\_defineeritud&gt;*

*&lt;mm:saatja\_defineeritud
mm:valjanimi="EhitusloaNr"&gt;2005-195&lt;/mm:saatja\_defineeritud&gt;*

*&lt;/dhl:metainfo&gt;*

*&lt;dhl:transport&gt;*

*&lt;dhl:saatja&gt;*

*&lt;dhl:regnr&gt;12345678&lt;/dhl:regnr&gt;*

*&lt;dhl:isikukood&gt;37501010001&lt;/dhl:isikukood&gt;*

*&lt;dhl:ametikoha\_kood&gt;171&lt;/dhl:ametikoha\_kood&gt;*

*&lt;dhl:ametikoha\_nimetus&gt;Noorembrigadir&lt;/dhl:ametikoha\_nimetus&gt;*

*&lt;dhl:epost&gt;hunt.kriimsilm@kuivendus.ee&lt;/dhl:epost&gt;*

*&lt;dhl:nimi&gt;Hunt Kriimsilm&lt;/dhl:nimi&gt;*

*&lt;dhl:asutuse\_nimi&gt;Kuivendusekspert OÜ&lt;/dhl:asutuse\_nimi&gt;*

*&lt;dhl:allyksuse\_kood&gt;10&lt;/dhl:allyksuse\_kood&gt;*

*&lt;dhl:allyksuse\_nimetus&gt;Planeerimisosakond&lt;/dhl:allyksuse\_nimetus&gt;*

*&lt;dhl:osakonna\_kood&gt;1&lt;/dhl:osakonna\_kood&gt;*

*&lt;dhl:osakonna\_nimi&gt;Planeerimisosakond&lt;/dhl:osakonna\_nimi&gt;*

*&lt;/dhl:saatja&gt;*

*&lt;dhl:saaja&gt;*

*&lt;dhl:regnr&gt;87654321&lt;/dhl:regnr&gt;*

*&lt;dhl:isikukood&gt;37005050005&lt;/dhl:isikukood&gt;*

*&lt;dhl:ametikoha\_kood&gt;411&lt;/dhl:ametikoha\_kood&gt;*

*&lt;dhl:ametikoha\_kood&gt;Peaspetsialist&lt;/dhl:ametikoha\_kood&gt;*

*&lt;dhl:epost&gt;karupoeg.puhh@vallavalitsus.ee&lt;/dhl:epost&gt;*

*&lt;dhl:nimi&gt;Karupoeg Puhh&lt;/dhl:nimi&gt;*

*&lt;dhl:asutuse\_nimi&gt;Vallavalitsus&lt;/dhl:asutuse\_nimi&gt;*

*&lt;dhl:allyksuse\_kood&gt;7&lt;/dhl:allyksuse\_kood&gt;*

*&lt;dhl:allyksuse\_nimetus&gt;Maaparandusosakond&lt;/dhl:allyksuse\_nimetus&gt;*

*&lt;dhl:osakonna\_kood&gt;MP&lt;/dhl:osakonna\_kood&gt;*

*&lt;dhl:osakonna\_nimi&gt;Maaparandusosakond&lt;/dhl:osakonna\_nimi&gt;*

*&lt;/dhl:saaja&gt;*

*&lt;/dhl:transport&gt;*

*&lt;dhl:ajalugu/&gt;*

*&lt;dhl:metaxml xmlns="http://www.riik.ee/schemas/dhl/rkel\_letter"*

*schemaLocation="http://www.riik.ee/schemas/dhl/rkel\_letter*

http://www.riik.ee/schemas/dhl/rkel\_letter.xsd"&gt;

*&lt;Addressees&gt;*

*&lt;Addressee&gt;*

*&lt;Organisation&gt;*

*&lt;organisationName&gt;Vallavalitsus&lt;/organisationName&gt;*

*&lt;departmentName&gt;Maaparandusosakond&lt;/departmentName&gt;*

*&lt;/Organisation&gt;*

*&lt;Person&gt;*

*&lt;firstname&gt;Karupoeg&lt;/firstname&gt;*

*&lt;surname&gt;Puhh&lt;/surname&gt;*

*&lt;jobtitle&gt;Peaspetsialist&lt;/jobtitle&gt;*

*&lt;email&gt;karupoeg.puhh@vallavalitsus.ee&lt;/email&gt;*

*&lt;telephone&gt;53 535 353&lt;/telephone&gt;*

*&lt;/Person&gt;*

*&lt;/Addressee&gt;*

*&lt;/Addressees&gt;*

*&lt;Author&gt;*

*&lt;Organisation&gt;*

*&lt;organisationName&gt;Kuivendusekspert OÜ&lt;/organisationName&gt;*

*&lt;departmentName&gt;Planeerimisosakond&lt;/departmentName&gt;*

*&lt;/Organisation&gt;*

*&lt;Person&gt;*

*&lt;firstname&gt;Hunt&lt;/firstname&gt;*

*&lt;surname&gt;Kriimsilm&lt;/surname&gt;*

*&lt;jobtitle&gt;Noorembrigadir&lt;/jobtitle&gt;*

*&lt;email&gt;hunt.kriimsilm@kuivendus.ee&lt;/email&gt;*

*&lt;telephone&gt;6 543 210&lt;/telephone&gt;*

*&lt;/Person&gt;*

*&lt;/Author&gt;*

*&lt;Signatures&gt;*

*&lt;Signature&gt;*

*&lt;Person&gt;*

*&lt;firstname&gt;Krokodill&lt;/firstname&gt;*

*&lt;surname&gt;Gena&lt;/surname&gt;*

*&lt;jobtitle&gt;Tegevjuht&lt;/jobtitle&gt;*

*&lt;email&gt;krokodill.gena@kuivendus.ee&lt;/email&gt;*

*&lt;telephone&gt;6 543 200&lt;/telephone&gt;*

*&lt;/Person&gt;*

*&lt;SignatureData&gt;*

*&lt;SignatureDate&gt;2006-03-11&lt;/SignatureDate&gt;*

*&lt;SignatureTime&gt;18:11:10&lt;/SignatureTime&gt;*

*&lt;/SignatureData&gt;*

*&lt;/Signature&gt;*

*&lt;Signature&gt;*

*&lt;Person&gt;*

*&lt;firstname&gt;Piilupart&lt;/firstname&gt;*

*&lt;surname&gt;Donald&lt;/surname&gt;*

*&lt;jobtitle&gt;Ehitusjärelvalve inspektor&lt;/jobtitle&gt;*

*&lt;email&gt;piilupart.donald@maavalitsus.ee&lt;/email&gt;*

*&lt;telephone&gt;77 43 210&lt;/telephone&gt;*

*&lt;/Person&gt;*

*&lt;SignatureData&gt;*

*&lt;SignatureDate&gt;2006-03-14&lt;/SignatureDate&gt;*

*&lt;SignatureTime&gt;23:59:59&lt;/SignatureTime&gt;*

*&lt;/SignatureData&gt;*

*&lt;/Signature&gt;*

*&lt;/Signatures&gt;*

*&lt;Compilators&gt;*

*&lt;Compilator&gt;*

*&lt;firstname&gt;Hunt&lt;/firstname&gt;*

*&lt;surname&gt;Kriimsilm&lt;/surname&gt;*

*&lt;jobtitle&gt;Noorembrigadir&lt;/jobtitle&gt;*

*&lt;email&gt;hunt.kriimsilm@kuivendus.ee&lt;/email&gt;*

*&lt;telephone&gt;6 543 210&lt;/telephone&gt;*

*&lt;/Compilator&gt;*

*&lt;/Compilators&gt;*

*&lt;LetterMetaData&gt;*

*&lt;SignDate&gt;2006-02-11&lt;/SignDate&gt;*

*&lt;SenderIdentifier&gt;A-101&lt;/SenderIdentifier&gt;*

*&lt;OriginalIdentifier&gt;A-101&lt;/OriginalIdentifier&gt;*

*&lt;Type&gt;Taotlus&lt;/Type&gt;*

*&lt;Language&gt;Eesti keel&lt;/Language&gt;*

*&lt;Title&gt;Kaeveloa taotlus, H. Kriimsilm&lt;/Title&gt;*

*&lt;Deadline&gt;2006-04-20&lt;/Deadline&gt;*

*&lt;Enclosures&gt;*

*&lt;EnclosureTitle&gt;Kraavi projekt&lt;/EnclosureTitle&gt;*

*&lt;EnclosureTitle&gt;Ehitusjärelvalve osakonna
luba&lt;/EnclosureTitle&gt;*

*&lt;/Enclosures&gt;*

*&lt;AccessRights&gt;*

*&lt;Restriction&gt;Asutusesiseseks kasutamiseks&lt;/Restriction&gt;*

*&lt;BeginDate&gt;2006-03-16&lt;/BeginDate&gt;*

*&lt;EndDate&gt;2011-03-16&lt;/EndDate&gt;*

*&lt;Reason&gt;Seadus §67 lg. 3&lt;/Reason&gt;*

*&lt;/AccessRights&gt;*

*&lt;/LetterMetaData&gt;*

*&lt;/dhl:metaxml&gt;*

*&lt;SignedDoc/&gt;*

*&lt;/dhl:dokument&gt;*


*&lt;dhl:dokument xmlns:dhl="http://www.riik.ee/schemas/dhl/2010/r1"
dhl:schemaLocation=“http://www.riik.ee/schemas/dhl.2010.r1.xsd“&gt;*

*&lt;dhl:metainfo
xmlns:ma="http://www.riik.ee/schemas/dhl-meta-automatic"
ma:schemaLocation=“*
*http://www.riik.ee/schemas/dhl/dhl-meta-automatic.2010.r1.xsd“*

*xmlns:mm="http://www.riik.ee/schemas/dhl-meta-manual"
mm:schemaLocation=“*
*http://www.riik.ee/schemas/dhl/dhl-meta-manual.2010.r1.xsd“&gt;*

*&lt;mm:koostaja\_asutuse\_nr&gt;87654321&lt;/mm:koostaja\_asutuse\_nr&gt;*

*&lt;mm:saaja\_asutuse\_nr&gt;99954321&lt;/mm:saaja\_asutuse\_nr&gt;*

*&lt;mm:koostaja\_dokumendinimi&gt;Avaldus
üritus&lt;/mm:koostaja\_dokumendinimi&gt;*

*&lt;mm:koostaja\_dokumendityyp&gt;Avaldus&lt;/mm:koostaja\_dokumendityyp&gt;*

*&lt;mm:koostaja\_dokumendinr&gt;A-101&lt;/mm:koostaja\_dokumendinr&gt;*

*&lt;mm:koostaja\_failinimi&gt;document.doc&lt;/mm:koostaja\_failinimi&gt;*

*&lt;mm:koostaja\_kataloog&gt;C:\\Dokumendid&lt;/mm:koostaja\_kataloog&gt;*

*&lt;mm:koostaja\_votmesona&gt;avaldus&lt;/mm:koostaja\_votmesona&gt;*

*&lt;mm:koostaja\_kokkuvote&gt;Avaldus massiürituse
korraldamiseks&lt;/mm:koostaja\_kokkuvote&gt;*

*&lt;mm:koostaja\_kuupaev&gt;2006-02-11T14:11:23+02:00&lt;/mm:koostaja\_kuupaev&gt;*

*&lt;mm:koostaja\_asutuse\_nimi&gt;Asutus
AS&lt;/mm:koostaja\_asutuse\_nimi&gt;*

*&lt;mm:koostaja\_asutuse\_kontakt&gt;Lauteri
21a&lt;/mm:koostaja\_asutuse\_kontakt&gt;*

*&lt;mm:autori\_osakond&gt;Haldusosakond&lt;/mm:autori\_osakond&gt;*

*&lt;mm:autori\_isikukood&gt;45604020013&lt;/mm:autori\_isikukood&gt;*

*&lt;mm:autori\_nimi&gt;Jaana Lind&lt;/mm:autori\_nimi&gt;*

*&lt;mm:autori\_kontakt&gt;51 22 762&lt;/mm:autori\_kontakt&gt;*

*&lt;mm:seotud\_dokumendinr\_koostajal&gt;XY-123&lt;/mm:seotud\_dokumendinr\_koostajal&gt;*

*&lt;mm:seotud\_dokumendinr\_saajal&gt;?Õ-ZŠ//Ä&lt;/mm:seotud\_dokumendinr\_saajal&gt;*

*&lt;mm:saatja\_dokumendinr&gt;AB-123&lt;/mm:saatja\_dokumendinr&gt;*

*&lt;mm:saatja\_kuupaev&gt;2006-02-12T01:02:03+02:00&lt;/mm:saatja\_kuupaev&gt;*

*&lt;mm:saatja\_asutuse\_kontakt&gt;Pärnu mnt.
176a&lt;/mm:saatja\_asutuse\_kontakt&gt;*

*&lt;mm:saaja\_isikukood&gt;38802280237&lt;/mm:saaja\_isikukood&gt;*

*&lt;mm:saaja\_nimi&gt;Eino Muidugi&lt;/mm:saaja\_nimi&gt;*

*&lt;mm:saaja\_osakond&gt;Sanitaarosakond&lt;/mm:saaja\_osakond&gt;*

*&lt;mm:saatja\_defineeritud mm:valjanimi="Koosolekul osaleja
1"&gt;Aksel&lt;/mm:saatja\_defineeritud&gt;*

*&lt;mm:saatja\_defineeritud mm:valjanimi="Koosolekul osaleja
2"&gt;Heldur&lt;/mm:saatja\_defineeritud&gt;*

*&lt;mm:saatja\_defineeritud mm:valjanimi="Koosolekul osaleja
3"&gt;Mari&lt;/mm:saatja\_defineeritud&gt;*

*&lt;mm:saatja\_defineeritud mm:valjanimi="Koosolekul osaleja
4"&gt;Loreida&lt;/mm:saatja\_defineeritud&gt;*

*&lt;mm:test&gt;false&lt;/mm:test&gt;*

*&lt;mm:dokument\_liik&gt;Kiri&lt;/mm:dokument\_liik&gt;*

*&lt;mm:dokument\_keel&gt;Eesti&lt;/mm:dokument\_keel&gt;*

*&lt;mm:dokument\_pealkiri&gt;Test
dokument&lt;/mm:dokument\_pealkiri&gt;*

*&lt;mm:versioon\_number&gt;1&lt;/mm:versioon\_number&gt;*

*&lt;mm:dokument\_guid&gt;cd171f7c-560d-4a62-8d65-16b87419a58c&lt;/mm:dokument\_guid&gt;*

*&lt;mm:dokument\_viit&gt;1.2/4-6&lt;/mm:dokument\_viit&gt;*

*&lt;mm:kuupaev\_registreerimine&gt;2010-02-12T01:02:03+02:00&lt;/mm:kuupaev\_registreerimine&gt;*

*&lt;mm:kuupaev\_saatmine&gt;2010-02-14T01:02:03+02:00&lt;/mm:kuupaev\_saatmine&gt;*

*&lt;mm:tahtaeg&gt;2010-02-28T01:02:03+02:00&lt;/mm:tahtaeg&gt;*

*&lt;mm:saatja\_kontekst&gt;*

*&lt;mm:seosviit&gt;1.1/5-7&lt;/mm:seosviit&gt;*

*&lt;mm:kuupaev\_saatja\_registreerimine&gt;2010-02-04T01:02:03+02:00&lt;/mm:kuupaev\_saatja\_registreerimine&gt;*

*&lt;mm:dokument\_saatja\_guid&gt;17084b40-08f5-4bcd-a739-c0d08c176bad&lt;/mm:dokument\_saatja\_guid&gt;*

*&lt;/mm:saatja\_kontekst&gt;*

*&lt;mm:ipr&gt;*

*&lt;mm:ipr\_tahtaeg&gt;2012-01-01T00:00:00+02:00&lt;/mm:ipr\_tahtaeg&gt;*

*&lt;mm:ipr\_omanik&gt;&lt;/mm:ipr\_omanik&gt;*

*&lt;mm:reprodutseerimine\_keelatud&gt;true&lt;/mm:reprodutseerimine\_keelatud&gt;*

*&lt;/mm:ipr&gt;*

*&lt;mm:juurdepaas\_piirang&gt;*

*&lt;mm:piirang&gt;Salastatud&lt;/mm:piirang&gt;*

*&lt;mm:piirang\_algus&gt;2009-01-01T00:00:00+02:00&lt;/mm:piirang\_algus&gt;*

*&lt;mm:piirang\_lopp&gt;2020-01-01T00:00:00+02:00&lt;/mm:piirang\_lopp&gt;*

*&lt;mm:piirang\_alus&gt;Riigisaladus&lt;/mm:piirang\_alus&gt;*

*&lt;/mm:juurdepaas\_piirang&gt;*

*&lt;mm:koostajad&gt;*

*&lt;mm:koostaja&gt;*

*&lt;mm:eesnimi&gt;Marko&lt;/mm:eesnimi&gt;*

*&lt;mm:perenimi&gt;Kurm&lt;/mm:perenimi&gt;*

*&lt;mm:ametinimetus&gt;Arendaja&lt;/mm:ametinimetus&gt;*

*&lt;mm:epost&gt;marko.kurm@microlink.ee&lt;/mm:epost&gt;*

*&lt;mm:telefon&gt;543322556&lt;/mm:telefon&gt;*

*&lt;/mm:koostaja&gt;*

*&lt;/mm:koostajad&gt;*

*&lt;/dhl:metainfo&gt;*

*&lt;dhl:transport&gt;*

*&lt;dhl:saatja&gt;*

*&lt;dhl:regnr&gt;87654321&lt;/dhl:regnr&gt;*

*&lt;dhl:nimi&gt;Jaak Lember&lt;/dhl:nimi&gt;*

*&lt;dhl:asutuse\_nimi&gt;Asutus&lt;/dhl:asutuse\_nimi&gt;*

*&lt;dhl:ametikoha\_nimetus&gt;Insenser&lt;/dhl:ametikoha\_nimetus&gt;*

*&lt;dhl:ametikoha\_lyhinimetus&gt;INSENER&lt;/dhl:ametikoha\_lyhinimetus&gt;*

*&lt;dhl:allyksuse\_nimetus&gt;Sepikoda&lt;/dhl:allyksuse\_nimetus&gt;*

*&lt;dhl:allyksuse\_lyhinimetus&gt;SEPIKODA&lt;/dhl:allyksuse\_lyhinimetus&gt;*

*&lt;/dhl:saatja&gt;*

*&lt;dhl:saaja&gt;*

*&lt;dhl:regnr&gt;99954321&lt;/dhl:regnr&gt;*

*&lt;dhl:asutuse\_nimi&gt;Maa-amet&lt;/dhl:asutuse\_nimi&gt;*

*&lt;dhl:allyksuse\_lyhinimetus&gt;SEPIKODA&lt;/dhl:allyksuse\_lyhinimetus&gt;*

*&lt;dhl:allyksuse\_nimetus&gt;Sepikoda&lt;/dhl:allyksuse\_nimetus&gt;*

*&lt;dhl:teadmiseks&gt;teadmiseks&lt;/dhl:teadmiseks&gt;*

*&lt;dhl:ametikoha\_nimetus&gt;Sepp&lt;/dhl:ametikoha\_nimetus&gt;*

*&lt;dhl:ametikoha\_lyhinimetus&gt;SEPP&lt;/dhl:ametikoha\_lyhinimetus&gt;*

*&lt;/dhl:saaja&gt;*

*&lt;/dhl:transport&gt;*

*&lt;dhl:ajalugu/&gt;*

*&lt;dhl:metaxml&gt;*

*...*

*&lt;/dhl:metaxml&gt;*

*&lt;dhl:failid&gt;*

*&lt;dhl:kokku&gt;1&lt;/dhl:kokku&gt;*

*&lt;dhl:fail&gt;*

*&lt;dhl:jrknr&gt;1&lt;/dhl:jrknr&gt;*

*&lt;dhl:fail\_pealkiri&gt;Test dokument&lt;/dhl:fail\_pealkiri&gt;*

*&lt;dhl:fail\_suurus&gt;779&lt;/dhl:fail\_suurus&gt;*

*&lt;dhl:fail\_tyyp&gt;text/plain&lt;/dhl:fail\_tyyp&gt;*

*&lt;dhl:fail\_nimi&gt;testdoc.txt&lt;/dhl:fail\_nimi&gt;*

*&lt;dhl:zip\_base64\_sisu&gt;...&lt;/dhl:zip\_base64\_sisu&gt;*

*&lt;dhl:krypteering&gt;false&lt;/dhl:krypteering&gt;*

*&lt;dhl:pohi\_dokument&gt;true&lt;/dhl:pohi\_dokument&gt;*

*&lt;/dhl:fail&gt;*

*&lt;/dhl:failid&gt;*

*&lt;dhl:konteineri\_versioon&gt;2&lt;/dhl:konteineri\_versioon&gt;\
&lt;/dhl:dokument&gt;*