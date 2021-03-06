# Dokumendivahetuskeskuse (DVK) tarkvara

DVK tarkvara koosneb klienditarkvarast ja serveritarkvarast. Samuti on olemas DVK API, mille abil saab liidestada DVK klienditarkvara kolmandate infosüsteemidega.

Repositooriumis on DVK tarkvaral viis erinevat alamprojekti:
 1. DVK-CORE - DVK tarkvara tuumikmeetodid ja parameetrifailid.
 2. DVK-CLIENT - DVK universaalkliendi tarkvara. Sõltub DVK-CORE projektist.
 3. DVK-SERVER - DVK serveri tarkvara. Sõltub DVK-CORE ja DVK-CLIENT projektist.
 4. DVK-API - DVK API tarkvara. DVK universaalkliendi ja kolmandate infosüsteemide liidestamiseks mõeldud teek.
 5. DOC - DVK tarkvara dokumentatsioon ning vastuvõtja staatuse id-d.
 6. INSTALLERS - DVK universaalkliendi installatsioonipaketid
 7. PORTAAL - Prototüüp DVK keskkonnast eesti.ee portaalis. 

DVK teenused ning päringute näited leiab [DVK spetsifikatsioonist](doc/DVKspek.md)

## X-tee versioon 6-le üleminek DVK kontekstis

DVK kasutaja jaoks kõige olulisem muudatuse seoses X-tee versioon 6-le üleminekul on muudatused X-tee sõnumiprotokollis (versioon 4.0). See tähendab, et DVK teenuste poole pöördumisel on vajalik seda teha senisest erinevate X-tee sõnumipäistega.
Täpsemad kirjeldused uutest päistest leiab [vastavast peatükist DVK spetsifikatsioonist](doc/DVKspek.md#x-tee-sõnumiprotokoll-versioon-40)


### Alamsüsteemide kasutamise loogika DVK-s

Kui asutus on DVK-ga liitunud mitme süsteemiga (näiteks eraldi struktuuriüksused, millele oleks võimalik otse dokumente adresseerida), siis on vaja v4.0 sõnumiprotokolli kasutades DVK keskserveris täpsustada alamsüsteemi (subsystemCode) ning registrikoodi (memberCode) väärtused. 

Varasemalt on selliste süsteemide puhul rakendatud nn X-tee 'alamsertifikaati', kus registrikoodi väärtusena kasutati 'alamsüsteem.registrikood' väärtust. X-tee uues versioonis aga saab registrikood (memberCode) sisaldada vaid päris registrikoodi väärtust ning struktuuriüksuse või infosüsteemi nimi tuleb defineerida alamsüsteemi (subsystemCode) väljal. Uuele sõnumiprotokollile üleminemisel soovitame alamsüsteemi nimeks valida see just see sama väärtus, mis varasemalt alamsertifikaadi puhul kasutati.   

Näiteks: <br>
- Viru Ringkonnaprokuratuur
```xml
<xrd:client id:objectType="SUBSYSTEM">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70000906</id:memberCode>
            <id:subsystemCode>viru.70000906</id:subsystemCode>
</xrd:client>
``` 

- Ametlike dokumentide infrastruktuuri teenus (ADIT)
```xml
<xrd:client id:objectType="SUBSYSTEM">
            <id:xRoadInstance>EE</id:xRoadInstance>
            <id:memberClass>GOV</id:memberClass>
            <id:memberCode>70006317</id:memberCode>
            <id:subsystemCode>adit</id:subsystemCode>
</xrd:client>
```  

Uuele sõnumiprotokollile üleminekul tuleb selliste süsteemide puhul anda teada RIA kasutajatoele (help@ria.ee), millise registrikoodi ja alamsüsteemiga päringuid soovitakse saata, ning eelnevalt testida testkeskkonnas, et kindlustada sujuvam üleminek. 

**Asutused, kes on varasemalt DVK-ga liidestunud ühe süsteemina (kasutanud selleks ühte asutuse X-tee sertifikaati) ning soovivad ka nii jätkata, ei pea uuele sõnumiprotokollile üleminemisel eraldi RIA kasutajatuge teavitama ning alamsüsteemi nimeks võib valida endale sobiva nimetuse, näiteks _'dok-haldus'_.**



DVK küsimuste puhul abistab Riigi Infosüsteemi Ameti kasutajatugi help@ria.ee
