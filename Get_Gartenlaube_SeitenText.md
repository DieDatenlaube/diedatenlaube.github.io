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
    {'pageid': 197749, 'title': 'Seite:Die Gartenlaube (1853) 011.jpg'}



    ---------------------------------------------------------------------------

    KeyboardInterrupt                         Traceback (most recent call last)

    /tmp/ipykernel_264/2522345768.py in <module>
         18         "format": "json"
         19         }
    ---> 20         R = S.get(url=URL, params=PARAMS)
         21         DATA = R.json()
         22         SeitenTextDet = {}


    /srv/paws/lib/python3.8/site-packages/requests/sessions.py in get(self, url, **kwargs)
        553 
        554         kwargs.setdefault('allow_redirects', True)
    --> 555         return self.request('GET', url, **kwargs)
        556 
        557     def options(self, url, **kwargs):


    /srv/paws/lib/python3.8/site-packages/requests/sessions.py in request(self, method, url, params, data, headers, cookies, files, auth, timeout, allow_redirects, proxies, hooks, stream, verify, cert, json)
        540         }
        541         send_kwargs.update(settings)
    --> 542         resp = self.send(prep, **send_kwargs)
        543 
        544         return resp


    /srv/paws/lib/python3.8/site-packages/requests/sessions.py in send(self, request, **kwargs)
        653 
        654         # Send the request
    --> 655         r = adapter.send(request, **kwargs)
        656 
        657         # Total elapsed time of the request (approximately)


    /srv/paws/lib/python3.8/site-packages/requests/adapters.py in send(self, request, stream, timeout, verify, cert, proxies)
        437         try:
        438             if not chunked:
    --> 439                 resp = conn.urlopen(
        440                     method=request.method,
        441                     url=url,


    /srv/paws/lib/python3.8/site-packages/urllib3/connectionpool.py in urlopen(self, method, url, body, headers, retries, redirect, assert_same_host, timeout, pool_timeout, release_conn, chunked, body_pos, **response_kw)
        697 
        698             # Make the request on the httplib connection object.
    --> 699             httplib_response = self._make_request(
        700                 conn,
        701                 method,


    /srv/paws/lib/python3.8/site-packages/urllib3/connectionpool.py in _make_request(self, conn, method, url, timeout, chunked, **httplib_request_kw)
        443                     # Python 3 (including for exceptions like SystemExit).
        444                     # Otherwise it looks like a bug in the code.
    --> 445                     six.raise_from(e, None)
        446         except (SocketTimeout, BaseSSLError, SocketError) as e:
        447             self._raise_timeout(err=e, url=url, timeout_value=read_timeout)


    /srv/paws/lib/python3.8/site-packages/urllib3/packages/six.py in raise_from(value, from_value)


    /srv/paws/lib/python3.8/site-packages/urllib3/connectionpool.py in _make_request(self, conn, method, url, timeout, chunked, **httplib_request_kw)
        438                 # Python 3
        439                 try:
    --> 440                     httplib_response = conn.getresponse()
        441                 except BaseException as e:
        442                     # Remove the TypeError from the exception chain in


    /usr/lib/python3.8/http/client.py in getresponse(self)
       1342         try:
       1343             try:
    -> 1344                 response.begin()
       1345             except ConnectionError:
       1346                 self.close()


    /usr/lib/python3.8/http/client.py in begin(self)
        305         # read until we get a non-100 response
        306         while True:
    --> 307             version, status, reason = self._read_status()
        308             if status != CONTINUE:
        309                 break


    /usr/lib/python3.8/http/client.py in _read_status(self)
        266 
        267     def _read_status(self):
    --> 268         line = str(self.fp.readline(_MAXLINE + 1), "iso-8859-1")
        269         if len(line) > _MAXLINE:
        270             raise LineTooLong("status line")


    /usr/lib/python3.8/socket.py in readinto(self, b)
        667         while True:
        668             try:
    --> 669                 return self._sock.recv_into(b)
        670             except timeout:
        671                 self._timeout_occurred = True


    /usr/lib/python3.8/ssl.py in recv_into(self, buffer, nbytes, flags)
       1239                   "non-zero flags not allowed in calls to recv_into() on %s" %
       1240                   self.__class__)
    -> 1241             return self.read(nbytes, buffer)
       1242         else:
       1243             return super().recv_into(buffer, nbytes, flags)


    /usr/lib/python3.8/ssl.py in read(self, len, buffer)
       1097         try:
       1098             if buffer is not None:
    -> 1099                 return self._sslobj.read(len, buffer)
       1100             else:
       1101                 return self._sslobj.read(len)


    KeyboardInterrupt: 


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
