# Get all the Gartenlaube Text in Wikisource

Mehr als 15.000 Artikel der Gartenlaube sind Ende 2021 in Wikisource erschlossen, ein vielfaches an Seiten der Illustrierten stehen hinter diesen Artikeln. 

## Frage 

Wie kommt man zum Texttranskript aller Gartenlaube-Artikel in Wikisource? (vgl. https://de.wikisource.org/wiki/Benutzer_Diskussion:Jeb#Gartenlaube_runterladen)

## Let's hack

Die MediaWiki-API bietet eine Vielzahl an Methoden um Daten aus MediaWikis - wie Wikisource - maschinell und im großen Stil zu extrahieren. Für große Projekte wie die Gartenlaube, die letztlich auch so sturkturiert sind wie sie strukturiert sind, geht es aber nicht ganz ohne Code. 

Was muss vorab beachtet werden?

* Der Text von Wikisource-Artikeln liegt nicht in den Seiten der strukturierten Zeitschriftenartikel, sondern in den Wiki-Artikel des `Seite:`-Namespaces
* Ein Wikisource-Großprojekt wie Die Gartenlaube ist nicht in einer einzigen Projektkategorie organisiert. Die einzelnen Seiten liegen in Jahrgangskategorien vor, die selbst eine Unterkategorie der Gartenlaube-Kategorie sind.
* Die MediaWiki-API bietet eine Extension an, um möglichst "Plain"-Text zu erhalten. Diese Extension `TextExctracts`(https://www.mediawiki.org/wiki/Extension:TextExtracts/de) ist aber für die Wikisource nicht verfügbar, da hier die Extension Proofread dies technisch gegenwärtig nicht ermöglicht. Daher ist ein Text-Output nur in einem gerenderte HTML oder im Wikitext möglich. Beide Varianten werden am Ende dieses Skripts im Output vereint.
* Mit der Python-Library `mwparserfromhell` wird noch zusätzlich ein plaintext ausgegeben, dieser Text basiert auf dem Wikicode, verliert aber sämtliche Textinhalte, die in Vorlagen gespeichert waren. Konkret sind dies Überschriften und Bildunterschriften. Dieser Volltext ist daher nur als Referenzwert zu verstehen und mit Vorsicht zu genießen.

### 1. Schritt - Alle Jahrgangskategorien parsen


```python
import requests

S = requests.Session()

URL = "https://de.wikisource.org/w/api.php"

PARAMS = {
    "action": "query",
    "cmtitle": "Kategorie:Die Gartenlaube",
    "cmlimit":500,
    "cmtype": "subcat",
    "list": "categorymembers",
    "format": "json"
}

R = S.get(url=URL, params=PARAMS)
DATA = R.json()

Gartenlaube_SubCat = DATA["query"]["categorymembers"]

#for SubCat in Gartenlaube_SubCat:
#    print(SubCat["title"])
```

### 2. Für jede Subkategorie der Gartenlaube, Seiten des "Seite:"-Namespaces abrufen


```python
pages = []
retDict = {}
for SubCat in Gartenlaube_SubCat:
    catPages = []
    PARAMS = {
    "action": "query",
    "cmtitle": SubCat["title"],
    "cmlimit":500,
    "cmtype": "page",
    "cmnamespace":102,
    "list": "categorymembers",
    "format": "json"
    }

    R = S.get(url=URL, params=PARAMS)
    DATA = R.json()

    Gartenlaube_Seiten = DATA["query"]["categorymembers"]

    for Pages in Gartenlaube_Seiten:
        pagedet = {}
        pagedet["pageid"] = Pages["pageid"]
        pagedet["title"] = Pages["title"]
        catPages.append(pagedet)
        #print(Pages["title"])

    try:
        PARAMS["cmcontinue"] = DATA["continue"]["cmcontinue"]
        R = S.get(url=URL, params=PARAMS)
        DATA = R.json()
        Gartenlaube_Seiten = DATA["query"]["categorymembers"]
        for Pages in Gartenlaube_Seiten:
            pagedet = {}
            pagedet["pageid"] = Pages["pageid"]
            pagedet["title"] = Pages["title"]
            catPages.append(pagedet)
            #print(Pages["title"])
    except KeyError:
        pass
    
    retDict[SubCat["title"]]=catPages
    
pages.append(retDict)
#print(pages)
```

### 3. Für jede Seite die zugehörigen Text auslesen

* Für jeden Jahrgang wird ein JSON-File erzeugt nach dem Muster `GartenlaubeSeitenText_{Jahrgangskategorie}_{Timestamp}.json`und beinhaltet ein JSON-Objekt mit folgender Struktur:
```json
 [{"pageid" : {PAGEID},
   "title"   : {PAGETITLE},
   "html"    : {HTML_OUTPUT},
   "wikitext": {WIKI_MARKUP},
   "plaintxt": {mwparserfromhell(WIKI_MARKUP).strip_code)}
   }]
 ```


```python
import mwparserfromhell
import time
import json

#print(pages[0]["Kategorie:Die Gartenlaube (1853)"])
#https://de.wikisource.org/w/api.php?action=parse&format=json&pageid=197745&prop=text%7Cwikitext

for page in pages[0]:
    SeitenText = []
    for wikisourcePage in pages[0][page]:
        print(wikisourcePage)
    
        PARAMS = {
        "action": "parse",
        "pageid": wikisourcePage["pageid"],
        "pageid": 197745,
        "prop":"text|wikitext",
        "format": "json"
        }
        R = S.get(url=URL, params=PARAMS)
        DATA = R.json()
        SeitenTextDet = {}
        SeitenTextDet["pageid"] = wikisourcePage["pageid"]
        SeitenTextDet["title"] = wikisourcePage["title"]
        SeitenTextDet["html"] = DATA["parse"]["text"]["*"]
        SeitenTextDet["wikitext"] = DATA["parse"]["wikitext"]["*"]
        wikicode = mwparserfromhell.parse(SeitenTextDet["wikitext"] )
        SeitenTextDet["plaintxt"] = wikicode.strip_code()
        SeitenText.append(SeitenTextDet)

    f = open("output/GartenlaubeSeitenText_"+page+"_"+str(time.time())[0:10]+".json", "w")
    print(json.dumps(SeitenText),file=f)
    f.close()      
        
```

    {'pageid': 180682, 'title': 'Seite:Die Gartenlaube (1853) 001.jpg'}
    {'pageid': 197740, 'title': 'Seite:Die Gartenlaube (1853) 002.jpg'}
    {'pageid': 197741, 'title': 'Seite:Die Gartenlaube (1853) 003.jpg'}
    {'pageid': 197742, 'title': 'Seite:Die Gartenlaube (1853) 004.jpg'}
    {'pageid': 197743, 'title': 'Seite:Die Gartenlaube (1853) 005.jpg'}
    {'pageid': 197744, 'title': 'Seite:Die Gartenlaube (1853) 006.jpg'}
    {'pageid': 197745, 'title': 'Seite:Die Gartenlaube (1853) 007.jpg'}
    {'pageid': 197746, 'title': 'Seite:Die Gartenlaube (1853) 008.jpg'}
    {'pageid': 197747, 'title': 'Seite:Die Gartenlaube (1853) 009.jpg'}
    {'pageid': 197748, 'title': 'Seite:Die Gartenlaube (1853) 010.jpg'}
    {...}


### 4. Merge der Jahrgangs-Files

* Die bestehenden Jahrgangs-Jsonfiles werden hier zu einer Gesamtdatei gemerged. 
* **Achtung**: Dieser Schritt läuft nur als python-Skript via Shell, nicht im Notebook.


```python
import glob
import time

read_files = glob.glob("/home/paws/Gartenlaube/output/*.json")
print(read_files)

with open("/home/paws/Gartenlaube/output/GartenlaubeSeiten_Text_"+str(time.time())[0:10]+".json", "wb") as outfile:
    outfile.write(b",".join([open(f, "rb").read() for f in read_files]))

```

<p>&nbsp;</p>
<p>Christian Erlinger, 9. Dezember 2021</p>
<img alt="Creative Commons Lizenzvertrag" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/80x15.png" />&nbsp;&nbsp;&nbsp;<a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Namensnennung 4.0 International Lizenz</a> <a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><script src="https://hypothes.is/embed.js" async></script>
