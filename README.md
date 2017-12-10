### Abstract

Negligering af sikkerhed på server i forbindelse med udvikling af et system, var skyld i massivt datatab og større nedbrud.
 
Store og populære systemer er mål for ondsindede hackere, som ønsker at stjæle data og afpresse ejerne til at betale (ransomware).
 
Med relativt simple midler, kan man sikre sig mod de mest benyttede angreb.
 
Trods sikring er der dog stadig mange faldgruber, og det er svært at sikre sig fuldstændigt.

___

# Manglende sikkerhed skyld i større nedbrud - *da hackernews-clone blev hacked*

Negligering af sikkerhed på server i forbindelse med udvikling af et system, var skyld i massivt datatab og større nedbrud. Udviklere bør generelt have et større fokus på sikkerhed.

> *“Din data er blevet downloadet, og sikret på vores servere. For at få din data tilbage – betal 0.2 BitCoin til vores BitCoin adresse. Kontakt os på email, og vedhæft bevis på betaling. Velbekomme!”*

![awkward seal](http://i0.kym-cdn.com/photos/images/original/000/741/861/6b4.jpg "Awkward Seal")

En alarm på vores monitoreringssystem havde gjort os opmærksomme på, at der var noget galt med vores system. Da alt pegede på at der var noget galt med databaseserveren, tog vi et kig på denne, og fandt førnævnte venlige besked som det eneste, i en database der tidligere havde indeholdt millioner af dokumenter.

### Årsag

En gennemgang af mulige årsager på hacket af vores database, gav os følgende punkter:
Database-URL har fremgået i kildekoden
Vores database-bruger-kode er blevet bruteforced
Vores database-port har været åben
Der har manglet en form for godkendelse af adgang til databasen

Resultatet viste sig en blanding af de to sidste punkter. Adgangen til vores database stod helt åben i vores firewall (port 27017), og oven i dette var der ingen generel sikkerhed sat op for at få adgang til hele databasen.

