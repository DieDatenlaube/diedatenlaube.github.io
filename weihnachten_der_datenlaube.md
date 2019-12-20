# Weihnachten der Datenlaube

Als "illustriertes Familienblatt" konnte sich auch die Gartenlaube nicht der Präsentation weihnachtlicher Inhalte verschließen. Lyrisches neben Rezensionen von Büchern für die Weihnachtszeit (und den familiären Gabentisch) oder Kulturgeschichtliches sowie Aufrufe zur Hilfe (["Weihnachtsbaum für unsere Ostsee-Deutschen"](https://de.wikisource.org/wiki/Ein_Weihnachtsbaum_f%C3%BCr_unsere_Ostsee-Deutschen)) lässt sich rund um die "stille Zeit" in der Gartenlaube lesen.
Die Identifizierung der [Liste](https://query.wikidata.org/#%23Query%20all%20Gartenlaube%20articles%20with%20a%20main%20subject%20Christmas%20or%20a%20facet%20or%20subclass%20of%20Christmas%20or%20without%20a%20subject%20but%20the%20string%20%22Weihnacht%22%20within%20its%20label%0ASELECT%20DISTINCT%20%3FDie_Gartenlaube%20%3FDie_GartenlaubeLabel%20%3Fzentrales_Thema%20%3Fzentrales_ThemaLabel%20%3Fimage%20WHERE%20%7B%0A%20%20%7B%0A%20%20%20%20%3FDie_Gartenlaube%20wdt%3AP1433%20wd%3AQ655617%3B%0A%20%20%20%20%20%20(wdt%3AP921%2B)%20%3Fzentrales_Thema%3B%0A%20%20%20%20%20%20wdt%3AP577%20%3Fpubdate%3B%0A%20%20%20%20%20%20wdt%3AP433%20%3Fissue%3B%0A%20%20%20%20%20%20wdt%3AP304%20%3Fpages.%0A%20%20%20%20OPTIONAL%20%7B%20%3FDie_Gartenlaube%20wdt%3AP18%20%3Fimage.%20%7D%0A%20%20%20%20%3Fzentrales_Thema%20(wdt%3AP31%2F(wdt%3AP279*))%20%3Fxmas.%0A%20%20%20%20%3Fxmas%20(wdt%3AP1269%2F(wdt%3AP279*))%20wd%3AQ19809.%0A%20%20%7D%0A%20%20UNION%0A%20%20%7B%0A%20%20%20%20%3FDie_Gartenlaube%20wdt%3AP1433%20wd%3AQ655617%3B%0A%20%20%20%20%20%20(wdt%3AP921%2B)%20%3Fzentrales_Thema%3B%0A%20%20%20%20%20%20wdt%3AP577%20%3Fpubdate%3B%0A%20%20%20%20%20%20wdt%3AP433%20%3Fissue%3B%0A%20%20%20%20%20%20wdt%3AP304%20%3Fpages.%0A%20%20%20%20BIND(wd%3AQ19809%20AS%20%3Fzentrales_Thema)%0A%20%20%20%20OPTIONAL%20%7B%20%3FDie_Gartenlaube%20wdt%3AP18%20%3Fimage.%20%7D%0A%20%20%7D%0A%20%20UNION%0A%20%20%7B%0A%20%20%20%20%3FDie_Gartenlaube%20wdt%3AP1433%20wd%3AQ655617%3B%0A%20%20%20%20%20%20wdt%3AP577%20%3Fpubdate%3B%0A%20%20%20%20%20%20wdt%3AP433%20%3Fissue%3B%0A%20%20%20%20%20%20wdt%3AP304%20%3Fpages.%0A%20%20%20%20FILTER(NOT%20EXISTS%20%7B%20%3FDie_Gartenlaube%20wdt%3AP921%20_%3Ab1.%20%7D)%0A%20%20%20%20OPTIONAL%20%7B%20%3FDie_Gartenlaube%20wdt%3AP18%20%3Fimage.%20%7D%0A%20%20%20%20%3FDie_Gartenlaube%20rdfs%3Alabel%20%3FDie_GartenlaubeLabel_.%0A%20%20%20%20FILTER(CONTAINS(%3FDie_GartenlaubeLabel_%2C%20%22Weihnacht%22%40de))%0A%20%20%7D%0A%20%20SERVICE%20wikibase%3Alabel%20%7B%20bd%3AserviceParam%20wikibase%3Alanguage%20%22%5BAUTO_LANGUAGE%5D%2Cen%22.%20%7D%0A%7D)  an weihnachtlichen Artikel findet über das Schlagwort sowie über den Stichwortfilter "Weihnacht*" im Titelfeld statt.

```
#Query all Gartenlaube articles with a main subject Christmas or a facet or subclass of Christmas or without a subject but the string "Weihnacht" within its label
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
    FILTER(NOT EXISTS { ?Die_Gartenlaube wdt:P921 _:b1. })
    OPTIONAL { ?Die_Gartenlaube wdt:P18 ?image. }
    ?Die_Gartenlaube rdfs:label ?Die_GartenlaubeLabel_.
    FILTER(CONTAINS(?Die_GartenlaubeLabel_, "Weihnacht"@de))
  }
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],en". }
}
```

Für die Schlagwortanalyse werden zwei Suchanfragen miteinander kombiniert: Einmal wird in den Artikel nach jenen Schlagwörtern gesucht, die als "Aspekt von" Weihnachten (z.B. Weihnachtsmusik) verlinkt sind und zusätzlich wird nach allen Begriffen, die in der direkten taxonomischen Folge des Begriffes Weihnachten auffindbar sind, gefiltert. 
Gut zu sehen ist, dass es auch noch einige weihnachtliche Beiträge gibt, die eine "Sacherschließung" benötigen. Ein kleine Weihnachtsgeschenk für #DieDatenlaube wäre: ein Schlagwort in einem Artikel-Item in Wikidata zu verlinken. Vielleicht finden sich sogar noch detaillierter beschreibende Begrifflichkeiten um die Vielfalt der im weihnachtlichen Kontext verwendeten Termini zu vergrößern. [BubbleChart]

## Wunschzettel 
* *Partizipation* Hilfe für Die Datenlaube https://blog.wikimedia.de/2019/10/16/hilfe-fuer-die-datenlaube-mit-wikisourcewikidata-die-freie-quellensammlung-verbessern/: [Schlagworte] oder [Genre] identifizieren und ergänzen.
* neue Ideen zur Datenqualität, weiteren Vernetzungen der Artikel und ihrer Datenobjekte, zu Abfragen und Visualisierungen beitragen oder diese und andere bearbeiten - hierfür verwenden wir jetzt ein Ticketsystem. die [Issues](https://github.com/DieDatenlaube/DieDatenlaube/issues) des Datenlaube GitHub-Repositories.

