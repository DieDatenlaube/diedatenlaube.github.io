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

for SubCat in Gartenlaube_SubCat:
    print(SubCat["title"])
```

    Kategorie:Die Gartenlaube Artikel
    Kategorie:Die Gartenlaube Hefte
    Kategorie:Die Gartenlaube (1853)
    Kategorie:Die Gartenlaube (1854)
    Kategorie:Die Gartenlaube (1855)
    Kategorie:Die Gartenlaube (1856)
    Kategorie:Die Gartenlaube (1857)
    Kategorie:Die Gartenlaube (1858)
    Kategorie:Die Gartenlaube (1859)
    Kategorie:Die Gartenlaube (1860)
    Kategorie:Die Gartenlaube (1861)
    Kategorie:Die Gartenlaube (1862)
    Kategorie:Die Gartenlaube (1863)
    Kategorie:Die Gartenlaube (1864)
    Kategorie:Die Gartenlaube (1865)
    Kategorie:Die Gartenlaube (1866)
    Kategorie:Die Gartenlaube (1867)
    Kategorie:Die Gartenlaube (1868)
    Kategorie:Die Gartenlaube (1869)
    Kategorie:Die Gartenlaube (1870)
    Kategorie:Die Gartenlaube (1871)
    Kategorie:Die Gartenlaube (1872)
    Kategorie:Die Gartenlaube (1873)
    Kategorie:Die Gartenlaube (1874)
    Kategorie:Die Gartenlaube (1875)
    Kategorie:Die Gartenlaube (1876)
    Kategorie:Die Gartenlaube (1877)
    Kategorie:Die Gartenlaube (1878)
    Kategorie:Die Gartenlaube (1879)
    Kategorie:Die Gartenlaube (1880)
    Kategorie:Die Gartenlaube (1881)
    Kategorie:Die Gartenlaube (1882)
    Kategorie:Die Gartenlaube (1883)
    Kategorie:Die Gartenlaube (1884)
    Kategorie:Die Gartenlaube (1885)
    Kategorie:Die Gartenlaube (1886)
    Kategorie:Die Gartenlaube (1887)
    Kategorie:Die Gartenlaube (1888)
    Kategorie:Die Gartenlaube (1889)
    Kategorie:Die Gartenlaube (1890)
    Kategorie:Die Gartenlaube (1891)
    Kategorie:Die Gartenlaube (1892)
    Kategorie:Die Gartenlaube (1893)
    Kategorie:Die Gartenlaube (1894)
    Kategorie:Die Gartenlaube (1895)
    Kategorie:Die Gartenlaube (1896)
    Kategorie:Die Gartenlaube (1897)
    Kategorie:Die Gartenlaube (1898)
    Kategorie:Die Gartenlaube (1899)
    Kategorie:Die Gartenlaube (1901)
    Kategorie:Die Gartenlaube (1920)
    Kategorie:Gartenlaube-Serie
    Kategorie:Kleiner Briefkasten


### 2. Für jede Subkategorie der Gartenlaube, Seiten des "Seite:"-Namespaces abrufen


```python
pages = []
for SubCat in Gartenlaube_SubCat:
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
        pages.append(pagedet)
        print(Pages["title"])

    try:
        PARAMS["cmcontinue"] = DATA["continue"]["cmcontinue"]
        R = S.get(url=URL, params=PARAMS)
        DATA = R.json()
        Gartenlaube_Seiten = DATA["query"]["categorymembers"]
        for Pages in Gartenlaube_Seiten:
            pagedet = {}
            pagedet["pageid"] = Pages["pageid"]
            pagedet["title"] = Pages["title"]
            pages.append(pagedet)
            print(Pages["title"])
    except KeyError:
        pass

```


### 3. Seiten-Text in HTML und Wikitext abrufen und lokal speichern


```python
#https://de.wikisource.org/w/api.php?action=parse&format=json&pageid=197745&prop=text%7Cwikitext
SeitenText = []
for page in pages:
    PARAMS = {
    "action": "parse",
    "pageid": page["pageid"],
    "prop":"text|wikitext",
    "format": "json"
    }
    R = S.get(url=URL, params=PARAMS)
    DATA = R.json()
    SeitenTextDet = {}
    SeitenTextDet["pageid"] = page["pageid"]
    SeitenTextDet["title"] = page["title"]
    SeitenTextDet["html"] = DATA["parse"]["text"]["*"]
    SeitenTextDet["wikitext"] = DATA["parse"]["wikitext"]["*"]
    SeitenText.append(SeitenTextDet)
    print(page["pageid"])
```

    
```python
import json

%store json.dumps(SeitenText) >>"Gartenlaube_SeitenText.json"
```

