### Abstract

Negligering af sikkerhed på server i forbindelse med udvikling af et system, var skyld i massivt datatab og større nedbrud.
 
Store og populære systemer er mål for ondsindede hackere, som ønsker at stjæle data og afpresse ejerne til at betale (ransomware).
 
Med relativt simple midler, kan man sikre sig mod de mest benyttede angreb.
 
Trods sikring er der dog stadig mange faldgruber, og det er svært at sikre sig fuldstændigt.

___

# Manglende sikkerhed skyld i større nedbrud - *da hackernews-clone blev hacked*

Negligering af sikkerhed på server i forbindelse med udvikling af et system, var skyld i massivt datatab og større nedbrud. Udviklere bør generelt have et større fokus på sikkerhed.

> *“Din data er blevet downloadet, og sikret på vores servere. For at få din data tilbage – betal 0.2 BitCoin til vores BitCoin adresse. Kontakt os på email, og vedhæft bevis på betaling. Velbekomme!”*

![warning](http://212.47.237.59:6001/test/blog/Screen%20Shot%202017-12-10%20at%2012.59.55.png "")

En alarm på vores monitoreringssystem havde gjort os opmærksomme på, at der var noget galt med vores system. Da alt pegede på at der var noget galt med databaseserveren, tog vi et kig på denne, og fandt førnævnte venlige besked som det eneste, i en database der tidligere havde indeholdt millioner af dokumenter.

### Årsag

En gennemgang af mulige årsager på hacket af vores database, gav os følgende punkter:
Database-URL har fremgået i kildekoden
Vores database-bruger-kode er blevet bruteforced
Vores database-port har været åben
Der har manglet en form for godkendelse af adgang til databasen

Resultatet viste sig en blanding af de to sidste punkter. Adgangen til vores database stod helt åben i vores firewall (port 27017), og oven i dette var der ingen generel sikkerhed sat op for at få adgang til hele databasen. Med disse to sikkerhedsbrister i opsætningen af vores firewall og database, var der direkte adgang til al data i vores database.

En hurtig googlesøgning vil dog afsløre, at vi langt fra er de eneste der har stået i denne situation. Mindst 30.000 databaser er blevet hacket på samme fremgangsmåde. Forskellige tools, såsom shodan.io, gør det nemt at fremskaffe IP-adresser, hvor MongoDB er installeret. Herefter er det blot at forbinde sig til serveren, da der som udgangspunkt ikke er godkendelse installeret på MongoDB-databaser.

### Fix

Heldigvis er det givne problem vi oplevede nemt at løse, det kræver blot at man lukker for adgang til porten i brug, eller definerer en specifik IP som gerne må få adgang gennem firewallen. Herudover findes der mange vejledninger til at få sat sikkerhed op i forbindelse med opsætning af sin database, som alle bør følge.

### Efterfølgende

En vigtig lektie i denne oplevelse, har været brugen af database-backups. Beslutningen om at lave backups af vores database, gjorde at vi kunne genskabe meget af den data, der ellers ville være gået tabt. Vi valgte at undlade at betale hackerne, da vi først og fremmest ikke kan være sikre på at de overhovedet ligger inde med den, og sekundært fordi det generelt anbefales af blandt andre Europol, at man ikke lader sig presse til at betale.

 
 ___
 
 ### Generelle Sikkerhedsbrister
 
Der er flere måder en hacker kan slippe ind. I langt de fleste tilfælde har en hacker fundet en brist i sikkerheden, eller nogen har ubevidst udsat systemet, f.eks. ved at være skødeløs med et password, åbne en ondsindet fil/mail eller indsat et ukendt externt drev (USB/DVD el. lign.). Angreb der faciliteres på denne måde kan være svære at forhindre, da der ikke er tale om en mangel i systemets sikkerhed, men derimod en person med utilstrækkelig viden omkring sikkerhed. 

![IT Securiy](https://i.imgur.com/tDikfo6.png "")

### Viden er magt
 
For at sikre sig imod angreb er man derfor nødt til, først of fremmest, at sikre sig at hele ens udvikler-team har en vis basisviden omkring de mest elementære sikkerhedstrusler. For en erfaren udvikler vil mange af disse trusler virke banale, men netop derfor er det vigtigt at have 100% styr på dem.

#### Trojans, Rootkits og andet malware

![Trojan Horse](http://s2.quickmeme.com/img/5d/5d91e23d0b04b87bc44a4068fda43ccead75a85e392fa6710812a6ca4459424f.jpg)

Mange af disse ondsindede programmer vil ofte inficere ens system via **mails**, ukendte **USB drev** eller i **downloads** forklædt som andre filer. Derfor er det altid god praktik at være skeptisk omkring disse medier. Man bør aldrig åbne en mail hvis autencitet man er den mindste smule i tvivl om. Ofte vil malware-inficerede mails være forklædt som mails fra pålidelige organisationer som Google eller Microsoft. Derfor kan det ved første øjekast være svært at vurdere om en mail er godartet eller ej. Man bør altid checke afsenderens email adresse, da hackerne ofte bruger adresser der til forveksling ligner de officielle organisationers, men som dog ikke er helt ens. Man bør også altid checke for stavefejl, da disse ofte kan afsløre en sløset bedrager. **Links** og **vedhæftede filer** bør aldrig åbne uden at have sikret sig afsenderens **pålidelighed**. 

Mange af de samme forholdsregler gør sig gældende for mobile drev (USB, DVD’er etc.) og downloads. Her bør man være ekstra påpasselig, og aldrig insertere et drev hvis ophav man ikke kender til. Med hensyn til downloads skal man naturligvis være kildekritisk og forsøge at holde sig til officielle, pålidelige organisationer. Hvis man har downloadet filer man frygter kan være ondsindede bør man altid skanne dem med et program, der kan registrere trusler (f.eks. Malwarebytes Anti Malware).

#### Data sikkerhed

Derudover er det vigtigt altid at være varsom omkring systemets data (om medarbejdere, brugere, transaktioner etc.) Får en hacker fat i et password kan det have store konsekvenser for en virksomhed. Er der tale om et password der giver adgang til dele af systemet vil hackeren nemt have mulighed for at forårsage yderligere skade. Er der tale om et password for et af systemets brugere, er det også ildevarslende. Folk bruger ofte det samme password flere steder, og hackeren vil derfor potentielt have mulighed for at få adgang til brugerens sociale medier, mail-konto eller i værste fald back-konto. Mister man en brugers password på den måde kan det være knusende for ens renomme.

Personfølsom data er også yderst delikat og bør aldrig deles skødesløst. Nyere lovgivning betyder at det kan være yderst dyrt at behandle personfølsom data forkert, og virksomheder der bryder lovgivningen på dette område blive tvunget til at betale et større millionbeløb i bøde.

### Konklusion

Viden omkring sikkerhed, og de praktikker og procedurer man bør overholde for at undgå sårbarheder, er fundamentale for enhver udvikler der vil undgå at udsætte sit system og dets brugere for trusler. Konsekvenserne af sløset sikkerhed er potentielt katastrofale, og kan i visse tilfælde nedlægge et system i en længere periode. 
