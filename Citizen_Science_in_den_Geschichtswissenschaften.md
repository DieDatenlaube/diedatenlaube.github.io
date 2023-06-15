# Citizen Science in den Geschichtswissenschaften

Im Februar 2021 wurden wir von [Frau Dr. Hendrikje Clarius](https://www.wikidata.org/wiki/Q111568731) eingeladen, "Die Datenlaube" in einem ausführlichen Beitrag als lebendiges bürger:innenwissenschaftliches Projekt mit geschichtswissenschaftlichen Bezügen in einem ausführlichen Beitrag für den geplanten Open Access Sammelband "[Citizen Science in den Geschichtswissenschaften](https://doi.org/10.14220/9783737015714)" vorzustellen. Dieser Einladung haben wir gerne Folge geleistet und etwas mehr als zwei Jahre später ist der Beitrag nun [abrufbar](https://doi.org/10.14220/9783737015714.163).

## Einmal mehr offene Metadaten für offene Publikationen

Was wir im Beitrag beschrieben haben, ist schlicht auch das, was wir eben einfach können. Beispielsweise bibliographische Einträge in Wikidata (semi-)maschinell erstellen und diese dann manuell-intellektuell kuratieren sowie ergänzen. Dieses Wissen und die damit verbundene intrinsische Motivation Wikidata bibliographisch zu befüllen, ist nicht nur auf die Gartenlaube beschränkt, sondern macht potentiell vor keiner Publikation halt. Da liegt es schlicht in der Natur der Sache, einen Sammelband mit Datenlaube-Inhalt zu "wikidatafizieren". Gesagt, getan. 

### Stapel stapeln – Datenlaube-Chat

<iframe src="Citizen Science in den Geschichtswissenschaften Chatprotokoll.html"  style="width: 100%; height: 680px; border: none;"></iframe>

### Let's hack and crack

Eigentlich also eine denkbar leichte Aufgabe. Auf der Übersichtsseite des Sammelbandes auf der Verlagswebsite <a href="https://www.vr-elibrary.de/doi/book/10.14220/9783737015714">doi:10.14220/9783737015714</a> ist alles drauf, was für einen schnellen Import in Wikidata nötig ist: Titel, Autor:innenanmen, DOI, Seitenzahl. Publikationsdatum, Sprache, Lizenz sind für alle Werke gleich. Die Elemente befinden sich in schön mit Klassen ausgezeichneten HTML-Tags – einfaches Web-Scraping mit Python also. Doch leider blockiert die Verlagswebsite sehr gezielt Seitenaufrufe maschineller Art. Auch verschiedene Tricks, den Header des Python-Requests möglichst wie einen Browser-Aufruf aussehen zu lassen, scheitern. Auch diverse [Python-Bibliothken](https://www.zenrows.com/blog/bypass-cloudflare-python), die vorgeben mit Cloudflare abgesicherte Websites einlesen zu können, kommen bei der Verlagswebsite nicht weiter. 

Da alle Informationen auf einer Seite gesammelt sind, schafft eine ganze banale Vorgehensweise Abhilfe: Öffnen des Quellcodes, kopieren in den Editor und lokal speichern. Mit der lokalen HTML-Datei klappt das Parsen des HTML-Codes und somit das Auslesen der genannten Informationen je Beitrag. 

```python
from bs4 import BeautifulSoup
from urllib.request import Request, urlopen
import urllib.request
import json
import re


resultSet = []
#rootHtml = getHTML("https://www.vr-elibrary.de/doi/book/10.14220/9783737015714#d135550e1313")
with open('SammelBand_CS.html',encoding='UTF-8') as f:
    contents = f.read()
    rootHtml = BeautifulSoup(contents, 'html.parser', from_encoding="utf-8")
    #print(contents)
    chapters = rootHtml.find_all("div", {'class': 'issue-item'})
    #print(chapters)
    for chapter in chapters:
        resultDet = {}
        #print(chapter)
        title = chapter.find("h5", {"class":"issue-item__title"}).find("a")
        resultDet["doi"] = (title["id"])
        resultDet["titel"] = (title["title"])
        
        pages = chapter.find("div", {"class":"issue-item__pages"})
        resultDet["pages"] =(pages.text)
        
        resultAuthors = []
        try:
            authors = chapter.find("div", {"class":"issue-item__loa"}).find_all("span")
            #print(authors)
            for author in authors:
                 if author.text.strip() != ",":
                        resultAuthors.append(author.text)
        except:
            pass
        resultDet["authors"] = resultAuthors
        
        #print(resultDet)
        resultSet.append(resultDet)

print(json.dumps(resultSet))
```

Titel, DOI, Seitenbereich und Autor:innennamen werden in als JSON ausgegeben und können dann in OpenRefine eingelesen und weiterverarbeitet werden. Denn es wollen ja die Autor:innen disambiguiert und wenn möglich mit bestehenden Wikidata-Items verlinkt werden. 

```json
[{"doi": "10.14220/9783737015714.front", "titel": "Titelei", "pages": "pp 1\u20134", "authors": []}, {"doi": "10.14220/9783737015714.toc", "titel": "Inhalt", "pages": "pp 5\u20136", "authors": []}, {"doi": "10.14220/9783737015714.7", "titel": "Citizen Science in den Geschichtswissenschaften aus methodischer       Perspektive: Zur Einf\u00fchrung", "pages": "pp 7\u201322", "authors": ["Ren\u00e9 Smolarski", "Hendrikje Carius", "Martin Prell"]}, {"doi": "10.14220/9783737015714.23", "titel": "Wie realistisch sind die Erwartungen an Citizen Science", "pages": "pp 23\u201340", "authors": ["Kristin Oswald"]}, {"doi": "10.14220/9783737015714.41", "titel": "Vom Crowdsourcing zu Co-Design", "pages": "pp 41\u201368", "authors": ["Tobias Hodel", "Christa Schneider"]}, {"doi": "10.14220/9783737015714.69", "titel": "Ber\u00fccksichtigung von Data-Literacy-Kompetenzen", "pages": "pp 69\u201390", "authors": ["Marina Lemaire", "Yvonne Rommelfanger"]}, {"doi": "10.14220/9783737015714.91", "titel": "Heimatforscher, Citizen Science und/oder Digital History?", "pages": "pp 91\u2013108", "authors": ["Katrin Moeller", "Moritz M\u00fcller"]}, {"doi": "10.14220/9783737015714.109", "titel": "Mehr als Zacken z\u00e4hlen?", "pages": "pp 109\u2013124", "authors": ["Ren\u00e9 Smolarski"]}, {"doi": "10.14220/9783737015714.125", "titel": "Vom Zettel zum Datensatz. Flurnamenforschung in Th\u00fcringen", "pages": "pp 125\u2013142", "authors": ["Barbara Aehnlich", "Petra Kunze"]}, {"doi": "10.14220/9783737015714.143", "titel": "Kultur und Geschichte Sachsens offen und kollaborativ erforschen", "pages": "pp 143\u2013162", "authors": ["Martin Munke"]}, {"doi": "10.14220/9783737015714.163", "titel": "Die Datenlaube \u2013 Citizen Science & digitale historische Hilfswissenschaft", "pages": "pp 163\u2013186", "authors": ["Jens Bemme", "Christian Erlinger"]}, {"doi": "10.14220/9783737015714.187", "titel": "Mythenbeschleuniger Oral History", "pages": "pp 187\u2013204", "authors": ["Elfi Vomberg"]}, {"doi": "10.14220/9783737015714.205", "titel": "Verderben viele K\u00f6che den Brei?", "pages": "pp 205\u2013222", "authors": ["Michael Brauer", "Marlene Ernst"]}, {"doi": "10.14220/9783737015714.223", "titel": "Crowdsourcing und Citizen Science mit Transkribus", "pages": "pp 223\u2013240", "authors": ["G\u00fcnter M\u00fchlberger", "Gerhard Siegl", "Kurt Scharr"]}, {"doi": "10.14220/9783737015714.241", "titel": "Keine Selbstverst\u00e4ndlichkeit: Citizen Science auf der FactGrid                               Wikibase-Plattform", "pages": "pp 241\u2013264", "authors": ["Olaf Simons"]}, {"doi": "10.14220/9783737015714.265", "titel": "Autorinnen- und Autorenverzeichnis", "pages": "pp 265\u2013269", "authors": []}]
```

### Identifier for everyone

Beim ersten Blick auf die Inhaltsübersicht des Sammelbandes fiel auf, dass nur die Herausgeber:innen mit ORCID identifizert sind. Alles klar – die Wikidata-Items bergen das Potential in sich das zu ändern. In der Tat konnten mit Ausnahme von zwei Autor:innen für alle (alle Autor:innen des Bandes besitzen nun eigene Wikidata-Items) Beitragenden ORCIDs identfiziert werden und wurden den Herausgeber:innen zur Korrektur an den Verlag gemeldet. Das ist idealtypisches [#DataRoundTripping](https://diff.wikimedia.org/2019/12/13/data-roundtripping-a-new-frontier-for-glam-wiki-collaborations/)  (wenn es der Verlag nachzieht).


## Web-Scraping als Fundament der Wikidata*fizierung

Web-Scraping ist in vielen Fällen eine grundlegende Technik um Web-Inhalte auszulesen und für den Import in Wikidata oder auch andere Datenbanken aufzubereiten. Das Sperren von Websites für skriptbasierte Aufrufe dient natürlich auch der Vermeidung von Risiken insbesonders von DDoS-Attacken, für "gut gemeinte" Zwecke wie den hier beschriebenen, ist das natürlich fatal. Auf openbiblio.social habe ich daher einen kleinen Aufruf gestartet, wie denn mit solchen Schwierigkeiten umgegangen werden soll:

<p><iframe src="https://openbiblio.social/@librerli/110536764637831880/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400" allowfullscreen="allowfullscreen"></iframe><script src="https://openbiblio.social/embed.js" async="async"></script></p>
<p><iframe src="https://openbiblio.social/@awinkler/110540311017565225/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400" allowfullscreen="allowfullscreen"></iframe><script src="https://openbiblio.social/embed.js" async="async"></script></p>
<iframe src="https://openbiblio.social/@librerli/110540884986981483/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400" allowfullscreen="allowfullscreen"></iframe><script src="https://openbiblio.social/embed.js" async="async"></script>

Der Hinweis von Alexander Winkler auf Crossref als alternative Quelle für die Datengrundlage ist für unser Beispiel hier natürlich ideal. Alle Daten der einzelnen Beiträge sind in Crossref bereits eingespielt und eine Abfrage aller lässt sich mit der Crossref-API und der ISBN bzw. der DOI für den Band leicht herstellen: [https://api.crossref.org/works?query=9783847115717](https://api.crossref.org/works?query=9783847115717). Es hätte keine weitere Bearbeitung in Python gebraucht, die API-Antwort könnte direkt als Datengrundlage in OpenRefine eingelesen werden.

Auf das Problem mit HTTP-Error-403-Antworten der V&R-Verlagswebsite bin ich allerdings schon vorher gestossen. Für die [ZHB Luzern](https://zhbluzern.ch) betreiben wir einen [Link-Checker](https://github.com/zhbluzern/LinkChecker), der überprüft, ob gewisse Open Access-Titel nach wie vor verfügbar sind. Die Fehlerliste bleibt durch die V&R-Website konstant befüllt, wie wohl kein Fehler vorliegt. Hierfür wäre noch eine gute Lösung gesucht oder vertrauen wir den "grossen Playern", die uns mit DOI "Permanenz" vorspielen?

<p>Christian Erlinger, 14. Juni 2023</p>
<img alt="Creative Commons Lizenzvertrag" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/80x15.png" />&nbsp;&nbsp;&nbsp;<a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Namensnennung 4.0 International Lizenz</a> <a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><script src="https://hypothes.is/embed.js" async></script></a>
<p>Wikidata-Item dieses Artikels: <a href="https://www.wikidata.org/wiki/Q119499972">(Q119499972)</a></p>