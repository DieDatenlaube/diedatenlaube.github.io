# O du fröhliche – weihnachtliche Gartenlaube

Als "illustriertes Familienblatt" konnte sich auch <em>Die Gartenlaube</em> weihnachtlichem <a href="https://de.wikisource.org/wiki/Weihnachten#Zeitschriftenartikel" target="_blank">Content</a> nicht verschließen. Warum auch? Lyrisches neben Rezensionen für erbauliche Literatur - für die Weihnachtszeit und den familiären Gabentisch - oder Kulturgeschichtliches sowie Aufrufe zur selbstlosen Hilfe (["Weihnachtsbaum für unsere Ostsee-Deutschen"](https://de.wikisource.org/wiki/Ein_Weihnachtsbaum_f%C3%BCr_unsere_Ostsee-Deutschen)) sind rund um die "stille Zeit" in der Gartenlaube zu finden und - im Falle der Spendenaufrufe und folgenden Spendenlisten - zu studieren.
Die Identifizierung der [Liste](https://w.wiki/EWy) weihnachtlicher Artikel funktioniert mit dem Schlagwort sowie über den Stichwortfilter "Weihnacht*" im Titelfeld.

```
#Query all Gartenlaube articles with a main subject Christmas or a facet or subclass of Christmas or without a subject but the string "Weihnacht or Christ/abend/tag/nacht" within its label
SELECT DISTINCT ?Die_Gartenlaube ?Die_GartenlaubeLabel ?zentrales_Thema ?zentrales_ThemaLabel ?image WHERE {
  {
    ?Die_Gartenlaube wdt:P1433 wd:Q655617;
      (wdt:P921+) ?zentrales_Thema;
      wdt:P577 ?pubdate;
      wdt:P433 ?issue;
      wdt:P304 ?pages.
    OPTIONAL { ?Die_Gartenlaube wdt:P18 ?image. }
    ?zentrales_Thema (wdt:P31/(wdt:P279*)) ?xmas.
    ?xmas (wdt:P1269/(wdt:P279*)) wd:Q19809.
  }
  UNION
  {
    ?Die_Gartenlaube wdt:P1433 wd:Q655617;
      (wdt:P921+) ?zentrales_Thema;
      wdt:P577 ?pubdate;
      wdt:P433 ?issue;
      wdt:P304 ?pages.
    BIND(wd:Q19809 AS ?zentrales_Thema)
    OPTIONAL { ?Die_Gartenlaube wdt:P18 ?image. }
  }
  UNION
  {
    ?Die_Gartenlaube wdt:P1433 wd:Q655617;
      wdt:P577 ?pubdate;
      wdt:P433 ?issue;
      wdt:P304 ?pages.
    #FILTER(NOT EXISTS { ?Die_Gartenlaube wdt:P921 _:b1. })
    OPTIONAL { ?Die_Gartenlaube wdt:P18 ?image. }
    ?Die_Gartenlaube rdfs:label ?Die_GartenlaubeLabel_.
    FILTER(REGEX(?Die_GartenlaubeLabel_, "Weihnacht|Christ(abend|nacht|tag)"@de))
  }
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],en". }
}
```
<iframe style="width: 80vw; height: 50vh; border: none; margin:20px;" src="https://query.wikidata.org/embed.html#SELECT%20DISTINCT%20%3FDie_Gartenlaube%20%3FDie_GartenlaubeLabel%20%3Fzentrales_Thema%20%3Fzentrales_ThemaLabel%20%3Fimage%20WHERE%20%7B%0A%20%20%7B%0A%20%20%20%20%3FDie_Gartenlaube%20wdt%3AP1433%20wd%3AQ655617%3B%0A%20%20%20%20%20%20(wdt%3AP921%2B)%20%3Fzentrales_Thema%3B%0A%20%20%20%20%20%20wdt%3AP577%20%3Fpubdate%3B%0A%20%20%20%20%20%20wdt%3AP433%20%3Fissue%3B%0A%20%20%20%20%20%20wdt%3AP304%20%3Fpages.%0A%20%20%20%20OPTIONAL%20%7B%20%3FDie_Gartenlaube%20wdt%3AP18%20%3Fimage.%20%7D%0A%20%20%20%20%3Fzentrales_Thema%20(wdt%3AP31%2F(wdt%3AP279*))%20%3Fxmas.%0A%20%20%20%20%3Fxmas%20(wdt%3AP1269%2F(wdt%3AP279*))%20wd%3AQ19809.%0A%20%20%7D%0A%20%20UNION%0A%20%20%7B%0A%20%20%20%20%3FDie_Gartenlaube%20wdt%3AP1433%20wd%3AQ655617%3B%0A%20%20%20%20%20%20(wdt%3AP921%2B)%20%3Fzentrales_Thema%3B%0A%20%20%20%20%20%20wdt%3AP577%20%3Fpubdate%3B%0A%20%20%20%20%20%20wdt%3AP433%20%3Fissue%3B%0A%20%20%20%20%20%20wdt%3AP304%20%3Fpages.%0A%20%20%20%20BIND(wd%3AQ19809%20AS%20%3Fzentrales_Thema)%0A%20%20%20%20OPTIONAL%20%7B%20%3FDie_Gartenlaube%20wdt%3AP18%20%3Fimage.%20%7D%0A%20%20%7D%0A%20%20UNION%0A%20%20%7B%0A%20%20%20%20%3FDie_Gartenlaube%20wdt%3AP1433%20wd%3AQ655617%3B%0A%20%20%20%20%20%20wdt%3AP577%20%3Fpubdate%3B%0A%20%20%20%20%20%20wdt%3AP433%20%3Fissue%3B%0A%20%20%20%20%20%20wdt%3AP304%20%3Fpages.%0A%20%20%20%20%23FILTER(NOT%20EXISTS%20%7B%20%3FDie_Gartenlaube%20wdt%3AP921%20_%3Ab1.%20%7D)%0A%20%20%20%20OPTIONAL%20%7B%20%3FDie_Gartenlaube%20wdt%3AP18%20%3Fimage.%20%7D%0A%20%20%20%20%3FDie_Gartenlaube%20rdfs%3Alabel%20%3FDie_GartenlaubeLabel_.%0A%20%20%20%20FILTER(REGEX(%3FDie_GartenlaubeLabel_%2C%20%22Weihnacht%7CChrist(abend%7Cnacht%7Ctag)%22%40de))%0A%20%20%7D%0A%20%20SERVICE%20wikibase%3Alabel%20%7B%20bd%3AserviceParam%20wikibase%3Alanguage%20%22%5BAUTO_LANGUAGE%5D%2Cen%22.%20%7D%0A%7D" referrerpolicy="origin" sandbox="allow-scripts allow-same-origin allow-popups"></iframe>

Für die Schlagwortanalyse kombinieren wir hier in Wikidata zwei Suchanfragen: Einmal wird in den Metadaten der bereits erschlossenen Gartenlaube-Artikel von 1853 bis 1899 nach jenen Schlagwörtern gesucht, die als "Aspekt von" Weihnachten (z.B. Weihnachtsmusik) verlinkt sind sowie zusätzlich wird nach allen Begriffen, die in der direkten taxonomischen Folge des Begriffes Weihnachten auffindbar sind, gefiltert. Dazu kommen noch Artikel ohne Schlagwörter, die aber die Stringfolge "Weihnacht" im Label beinhalten.
Gut zu sehen ist in der Abfrage, dass noch einige Weihnachtsbeiträge eine "Sacherschließung" benötigen: mit Schlagworten, die den Inhalt der Texte gut beschreiben. Ein nützliches Weihnachtsgeschenk für #DieDatenlaube wäre also, ein paar Schlagworte in Artikel-Items in Wikidata zu verlinken (<em>main subject</em>). Vielleicht finden sich sogar noch präziser beschreibende Begriffe, um die Vielfalt der im weihnachtlichen Kontext verwendeten Termini weiter zu vergrößern!? Wir nennen das <a href="https://w.wiki/3Rj" target="_blank"><em>Bubble Chart</em> aller vergebenen Schlagworte</a> in Tweets auch <a href="https://twitter.com/hashtag/Baumscheibendiagramm" target="_blank">(Christ-)Baumscheibendiagramm</a>.

Ganz wunschlos sind wir also nicht. Die Arbeit an der Datenlaube hat gerade erst begonnen (2019). Die Arbeiten an der <a href="https://de.wikisource.org/wiki/Die_Gartenlaube" target="_blank">Gartenlaube in Wikisource</a> laufen schon ein paar Jahre. Sie sind für uns die Basis. Allen, die hier miteifern, wünschen wir: Schöne Bescherung!  

## Unser Wunschzettel 
* Hilfe für <a href="https://blog.wikimedia.de/2019/10/16/hilfe-fuer-die-datenlaube-mit-wikisourcewikidata-die-freie-quellensammlung-verbessern/" target="_blank"><em>Die Datenlaube</em></a>: Schlagworte oder Genre identifizieren und in den Artikel-Items ergänzen.
* Neue Ideen für die Datenqualität, weitere Vernetzungen der Artikel und ihrer Datenobjekte, Beiträge zu neuen und alten <a href="https://de.wikisource.org/wiki/Wikisource:Wikidata#Die_Gartenlaube" target="_blank">Abfragen</a> und deren Visualisierungen.
* Tickets für unser Ticketsystem und deren Bearbeitung: vgl. die <a href="https://github.com/DieDatenlaube/DieDatenlaube/issues" target="_blank"><em>Issues</em></a> im Datenlaube-GitHub-Repository.
* Neue Open Educational Ressources auf Basis der Gartenlaube-Texte und Queries, z.B. von HistoDigitaLE <a href="https://de.wikisource.org/wiki/Wikisource:OER" target="_blank">aus der Uni Leipzig</a>.
* <em>Bunt ist schön, schwarz/weiß ist dramatisch!</em>, lehren einen Fotografen. Trotzdem hier die Frage: Wer coloriert bitte <a href="https://commons.wikimedia.org/wiki/File:Die_Datenlaube.xcf" target="_blank">unser Logo</a>?!
* Die Datenlaube möge als Blaupause helfen auch die anderen Textcorpora in Wikisource mittels Wikidata noch besser zugänglich und benutzbar zu machen. Deshalb ist das unser Lesetipp für die Feiertage: <a href="https://space.wmflabs.org/2019/12/07/how-can-structured-data-on-commons-wikidata-and-wikisource-walk-hand-in-hand-a-pilot-project-with-punjabi-qisse/"><em>How can Structured Data on Commons, Wikidata, and Wikisource walk hand in hand? A pilot project with Punjabi Qisse</em></a>.

Frohes Fest!

wünscht das Team [<em>#DieDatenlaube</em>](https://diedatenlaube.github.io/die_datenlaube_der_gartenlaube)

<p>&nbsp;</p>
<p>&nbsp;</p>
<p>Veröffentlicht am 22.12.2019</p>
<img alt="Creative Commons Lizenzvertrag" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/80x15.png" />&nbsp;&nbsp;&nbsp;<a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Namensnennung 4.0 International Lizenz</a> <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">
