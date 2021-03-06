<h1 id="die-datenlaube-der-gartenlaube">Die Datenlaube der Gartenlaube</h1>
<p><strong>Mit Wikidata ‚Die Gartenlaube‘ in Wikisource strukturiert erschließen – ein Werkstattbericht</strong></p>

<p><em>Christian Erlinger (</em><a href="https://www.wikidata.org/wiki/Q67173261"><em>Q67173261</em></a><em>)</em><br/>
<em>Jens Bemme (</em><a href="https://www.wikidata.org/wiki/Q56880673"><em>Q56880673</em></a><em>)</em></p>

<p>Eines der umfangreichsten Projekte der deutschsprachigen <a href="https://de.wikisource.org/">Wikisource</a>-Community ist die Bearbeitung und Tiefenerschließung der ersten großen deutschsprachigen Illustrierten „<a href="https://de.wikisource.org/wiki/Die_Gartenlaube">Die Gartenlaube</a>‟. </p>

<p>Seit Beginn des Jahres 2019 werden die transkribierten Artikel dieser Zeitschrift vollständig mit Hilfe von Wikidata formal und inhaltlich erschlossen.<a href="#fn1" class="footnote-ref" id="fnref1" role="doc-noteref"><sup>1</sup></a> Wikidata wird dadurch zur offenen und strukturierten Bibliographie der freien Quellensammlung Wikisource. Die Methoden dieser teilautomatisierten bibliographischen Datenextraktion und Erschließung mit verschiedenen Tools in Wikidata wie <a href="https://tools.wmflabs.org/quickstatements/"><em><em>QuickStatements</em></em></a> oder <a href="https://jupyter.org/"><em><em>Jupyter Notebooks</em></em></a> sind im Folgenden dokumentiert.</p>

<p>Von geschätzt 18.500 Artikel, die ab 1853 bis zum Jahr 1900 in der Gartenlaube erschienen sind, sind per 1. November 2019 bereits 12.990 Artikel in Wikisource vorhanden – basierend auf 40.366 gescannten Seiten: gescannt, transkribiert oder auf Basis der Volltexterkennung (OCR) korrigiert.<a href="#fn2" class="footnote-ref" id="fnref2" role="doc-noteref"><sup>2</sup></a></p>

<p>Die Artikel in Wikisource werden allesamt mit einer für Mediawikiprojekte typischen Infobox mit grundlegenen Metadaten ausgestattet. (vgl. Abb. 1) Dies umfasst in der Regel den Titel des Artikels, die Quellenstelle mit Heftnummer, Seitenzahl(en) und das Publikationsjahr. Gegebenenfalls ist auch ein Kurzzusammenfassung und ein Link zu einer relevanten Wikipedia-Seite angegeben, die ggf. das Hauptthema des Textes beschreibt und somit ein erstes Schlagwort darstellt. Darüber hinaus beinhaltet die Infobox noch Links zu den Seitenscans sowie Informationen zum Bearbeitungsstand der Texterschließung nach den Vorgaben der Wikisource-Community. All diese in gewisser Weise „strukturierten‟ (aber nur bedingt maschinenlesbaren) Daten der Wikisource-Seite lassen sich dann in einem Wikidata-Item (vgl. Abb. 2) strukturiert verankern und vor allem für Menschen wie Maschinen gleichermaßen les- und durchsuchbar halten.</p>

<figure>
<img src="./Pictures/10000201000000F50000025BBC6E3BEF99879393.png" style="width:6.482cm;height:15.954cm" alt="" /><figcaption><br />
Abbildung 1: Infobox des Gartenlaubeartikels <a href="https://de.wikisource.org/wiki/Der_Volkspalast_im_Ostend_von_London">"Der Volkspalast im Ostend von London."</a></figcaption>
</figure>

<figure>
<img src="./Pictures/1000020100000466000003426A75ECA0964D77FF.png" style="width:17cm;height:12.591cm" alt="" /><figcaption><br />
Abbildung 2: Das Frontend des „ältesten‟ Wikidata-Items eines Gartenlaube-Artikels: „Der Volkspalast im Ostend von London‟ (<a href="https://www.wikidata.org/w/index.php?title=Q15892076&amp;oldid=943569455">Q15892076</a> [14.11.2014]) zeigt mehrsprachige Labels, Artikel-Statement, Bild-Verlinkung und Titel</figcaption>
</figure>

<h2 id="datenmodell">Datenmodell</h2>
<p>Wie in allen Portalen des Wiki*versums ist auch in Wikisource die Möglichkeit gegeben, die jeweiligen Wikisource-Pages mit einem spezifischen Datenobjekt (Item) in Wikidata zu verlinken. Im Fall der Gartenlaube bedeutet dies, dass für jeden Artikel ein Wikidata-Item als bibliographischer Datensatz angelegt wird. Ein solches Item sollte ein entsprechendes Mindestset an bibliographischen Informationen des Artikels bereithalten. Das genutzte Basis-Metadatenschma ist in der nachfolgenden Tabelle dargestellt.</p>

<table>
<thead>
<tr>
<th>Property/Wikidata-Metadatenfeld</th>
<th>Format/Beschreibung des Inhalts</th>
<th>Beispiel <a href="https://www.wikidata.org/wiki/Q61996511">Q61996511</a> </th>
</tr>
</thead>
<tbody>
<tr class="even">
<td>label_de</td>
<td>Titel des Artikels (Deutsch)</td>
<td>Jean Paul Richter </td>
</tr>
<tr class="odd">
<td>label_en</td>
<td>Titel des Artikels (English)</td>
<td>Jean Paul Richter </td>
</tr>
<tr class="even">
<td>description_de</td>
<td>Arikel in: Zeitschrift, Jahrgang, Nr.</td>
<td>Artikel in: Die Gartenlaube, 1853, Heft 34 </td>
</tr>
<tr class="odd">
<td>description_en</td>
<td>german article in Journal, Volume, Issue</td>
<td>german article in Die Gartenlaube, 1853, no. 34 </td>
</tr>
<tr class="even">
<td><a href="https://www.wikidata.org/wiki/Property:P31">P31</a> instance of</td>
<td></td>
<td>article <a href="https://www.wikidata.org/wiki/Q191067">Q191067</a> </td>
</tr>
<tr class="odd">
<td><a href="https://www.wikidata.org/wiki/Property:P1476">P1476</a> title</td>
<td>Titel des Artikels</td>
<td>Jean Paul Richter </td>
</tr>
<tr class="even">
<td><a href="https://www.wikidata.org/wiki/Property:P407">P407</a> language of work or name</td>
<td></td>
<td>German <a href="https://www.wikidata.org/wiki/Q188">Q188</a> </td>
</tr>
<tr class="odd">
<td><a href="https://www.wikidata.org/wiki/Property:P577">P577</a> publication date</td>
<td>YYYY</td>
<td>1853 </td>
</tr>
<tr class="even">
<td><a href="https://www.wikidata.org/wiki/Property:P304">P304</a> pages</td>
<td></td>
<td>197 </td>
</tr>
<tr class="odd">
<td><a href="https://www.wikidata.org/wiki/Property:P433">P433</a> issue</td>
<td></td>
<td>18 </td>
</tr>
<tr class="even">
<td><a href="https://www.wikidata.org/wiki/Property:P1433">P1433</a> published in</td>
<td>Journal</td>
<td>Die Gartenlaube <a href="https://www.wikidata.org/wiki/Q655617">Q655617</a> </td>
</tr>
<tr class="odd">
<td><a href="https://www.wikidata.org/wiki/Property:P921">P921</a> main subject</td>
<td></td>
<td>Jean Paul <a href="https://www.wikidata.org/wiki/Q77079">Q77079</a> </td>
</tr>
<tr class="even">
<td>dewikisource_sitelink</td>
<td>Titel des Artikels</td>
<td><a href="https://de.wikisource.org/wiki/Jean_Paul_Richter">Jean Paul Richter</a></td>
</tr>
</tbody>
</table>
<div class="caption">
<p>Tabelle 1: Basis-Metadatenmodell für Gartenlaube-Artikel in Wikidata<a href="#fn3" class="footnote-ref" id="fnref3" role="doc-noteref"><sup>3</sup></a></p>
</div>

<p>Die Flexibilität und Offenheit des Datenmodells in Wikidata insgesamt erlaubt es für die spezifischen Items weitere Statements zu ergänzen, wie beispielhaft in der Tabelle 2 angeführt. Dies wäre bspw. die Ergänzung und Verlinkung von Illustrationen, der Nennung eines Illustrators, sofern auffindbar Links in Bibliothekskataloge zu den entsprechenden lokalen bibliographischen Fundstellen oder schlicht was sonst noch denkbar und möglich erscheint oder dereinst – durch Erzeugung neuer Wikidata-Properties für formale und inhaltliche Erschließung – möglich sein wird.</p>

<table>
<thead>
<tr>
<th>Property/Wikidata-Metadatenfeld</th>
<th>Format/Beschreibung des Inhalts</th>
<th>Beispiel <a href="https://www.wikidata.org/wiki/Q61996511">Q61996511</a> </th>
</tr>
</thead>
<tbody>
<tr class="even">
<td>label_[Multiple_Languages]</td>
<td>Titel des Artikels in weiteren Sprachen – Tendenziell im Original, gegebenenfalls transliteriert.</td>
<td>Jean Paul Richter </td>
</tr>
<tr class="odd">
<td>description_[Multiple_Languages]</td>
<td>Beschreibung des Items in weiteren Sprachen</td>
<td>Artikel in: Die Gartenlaube, 1853, Heft 34 </td>
</tr>
<tr class="even">
<td><a href="https://www.wikidata.org/wiki/Property:P136">P136</a> genre</td>
<td>Literaturgattung</td>
<td>poem <a href="https://www.wikidata.org/wiki/Q5185279">Q5185279</a> </td>
</tr>
<tr class="odd">
<td><a href="https://www.wikidata.org/wiki/Property:P18">P18</a> image</td>
<td>Illustrationen – direkte Verlinkung mit Bild auf Commons</td>
<td></td>
</tr>
<tr class="even">
<td><a href="https://www.wikidata.org/wiki/Property:P110">P</a><a href="https://www.wikidata.org/wiki/Property:P110">110</a> illustrator</td>
<td>Illustrator eines Werks</td>
<td>German <a href="https://www.wikidata.org/wiki/Q188">Q188</a> </td>
</tr>
<tr class="odd">
<td><a href="https://www.wikidata.org/wiki/Property:P1343">P1343</a><a href="https://www.wikidata.org/wiki/Property:P1343"> </a>described by source</td>
<td>Externe Fundstelle</td>
<td>Regional bibliography of Saxony <a href="https://www.wikidata.org/wiki/Q61729277">Q61729277</a></td>
</tr>
<tr class="even">
<td><a href="https://www.wikidata.org/wiki/Property:P996">P</a><a href="https://www.wikidata.org/wiki/Property:P996">996</a> scanned file on Wikimedia Commons</td>
<td>Verlinkung der Quelltext-Scans auf Commons</td>
<td> </td>
</tr>
<tr class="odd">
<td>[wikiproject]_sitelink</td>
<td>Sitelinks zu weiteren Wiki*Projekten (z.B. Wikipedia-Link zum Werk)</td>
<td><a href="https://de.wikisource.org/wiki/Jean_Paul_Richter"></a></td>
</tr>
</tbody>
</table>
<div class="caption">
<p>Tabelle 2: Beispiele weiterer [optionaler] Eigenschaften eines bibliographischen Items der Gartenlaube</p>
</div>
<p>Die Verknüpfung und Erschließung von Wikisource mittels Wikidata in Projekten mit historischen Texten (unabhängig davon, ob es sich um selbständige oder unselbständige Literatur handelt) ist ein erster Schritt. Darauf aufbauend kann weiteres Augenmerk auf ein drittes Wikimediaportal gelegt werden, ohne das die Volltexterschließung auf Wikisource so nicht möglich wäre: Wikimedia Commons. Auf <a href="https://commons.wikimedia.org/">Commons</a> sind sämtliche für Wikisourceprojekte notwendigen Quelldaten gespeichert, die rohen Scans der Einzelseiten sowie extrahierte und bearbeitete Illustrationen. Insbesondere durch die Etablierung von <a href="https://commons.wikimedia.org/wiki/Commons:Structured_data/"><em>Structured Data On Commons</em></a> – der Einbindung von Linked Data nach dem Vorbild und unter Einbeziehung von Wikidata – ergeben sich hier seit dem Frühjahr 2019 neue und vor allem langfristig sehr nützlich erscheinende Beziehung.<a href="#fn4" class="footnote-ref" id="fnref4" role="doc-noteref"><sup>4</sup></a> In Wikidata erzeugte bibliographische Items können nun in den Commons als stabiler Link bzw. Fundstelle für die jeweils in den Zeitungsartikeln vorhandenen Illustrationen oder Seitenscans verwendet werden. Das Nachweissystem für die Scans als Quelldokumente der folgenden Artikeltranskriptionen würde damit noch stabiler.</p>
<h2 id="anzahl-und-qualität-der-artikel-items-in-wikidata-beim-projektstart-2019">Anzahl und Qualität der Artikel-Items in Wikidata beim Projektstart 2019</h2>
<p>Per 1. März 2019 hatten 7.599 Artikel der Gartenlaube ein verlinktes Item in Wikidata. Eine SPARQL-Abfrage nach Artikeln mit dem Statement „published in‟ „Die Gartenlaube‟ ist aber mit den Daten des damaligen Zeitpunktes nicht möglich, da den vorhandenen Items als Veröffentlichungsort nicht die Zeitschrift selbst, sondern ein Jahrgangs-Item (z.B. „<a href="https://www.wikidata.org/wiki/Q19175246">Gartenlaube 1878</a>‟) als Fundstelle eingetragen wurde. Der Wert an bereits vorhandenen Wikidata-Items der Gartenlaube errechnet sich aus der gegenwärtig verfügbaren Liste aller Wikidata-Items unter nummerischer Auswertung der Q-ID der Wikidata-Items,<a href="#fn5" class="footnote-ref" id="fnref5" role="doc-noteref"><sup>5</sup></a> um auf ein enstprechend frühes Anlagedatum des Items zu schließen (Konkret handelt es sich dabei um alle Gartenlaube-Items mit einer ID kleiner Q50000000).<a href="#fn6" class="footnote-ref" id="fnref6" role="doc-noteref"><sup>6</sup></a></p>


<figure>
<img src="./Pictures/100002010000036C00000316CE8EE444B938316A.png" style="width:17cm;height:11.92cm" alt="" /><figcaption><br />
Abbildung 3: Status des „ältesten‟ Wikidata-Items des Gartenlaube-Artikels „Der Volkspalast im Ostend von London‟ (<a href="https://www.wikidata.org/wiki/Q15892076">Q15892076</a>) nach seiner Anlage am 05.03.2014</figcaption>
</figure>
<br><br>
<p>Der Großteil dieser mehr als 7.000 Items war hinsichtlich der bibliographischen Beschreibung eher dürftig, da es sich dabei um eine semi-automatische Anlage mittels des Tools <a href="https://petscan.wmflabs.org/">PetScan</a> oder automatisiert per Bot handelte, bei der auf Basis der jeweiligen Jahrgangskategorie in Wikisource Wikidata-Items nur rudimentär angelegt wurden (wie für das älteste Gartenlaube-Item in Abbildung 3 gezeigt).</p>
<figure>
<img src="./Pictures/100002010000059C000004335DEB63C25E419F99.png" style="width:17cm;height:12.053cm" alt="" /><figcaption><br />
Abbildung 4: Verteilung der Anzahl an Items je Anzahl an Statements der Gartenlaube Artikel</figcaption>
</figure>

<p>Die zum Zeitpunkt 9. März 2019 vorhandenen Items hatten im Schnitt zwei Statements.<a href="#fn7" class="footnote-ref" id="fnref7" role="doc-noteref"><sup>7</sup></a> Die beiden am häufigst eingesetzten Properties waren dabei P31 (<em>„instance of‟</em>) und P1433 (<em>„published in‟</em>) wie der Tabelle 3 zu entnehmen. Wie aus Abbildung 4 ersichtlich hatten 6.516 Items weniger oder gleich zwei Statements. Bei Items mit einer sehr hohen Anzahl an Statements wie all jenen mit mehr als 20 Statements lohnte sich der genaue Blick auf das Item, da es sich hierbei dann durchwegs um falsche Zuordnungen des Wikisource-Sitelinks des Gartenlaubeartikels zu einem Wikidata-Item handelte. Beispielsweise wurden biographische Artikel direkt dem biographischen Item, d.h. der Person in Wikidata zugeordnet. Eine in einem Artikel beschriebene Person kann respektive soll aber immer nur als verlinktes Schlagwort (<em>„main subject‟</em>) im Artikel-Item verwendet werden. <br/>Auf Basis dieser rudimentären Analyse der vorhandenen Einträge war klar, dass nicht nur die fehlenden Items dem neuen Datenmodell gemäß anzulegen sind, sondern auch die große Zahl der bestehenden Items einer gründlichen Überarbeitung bzw. Ergänzung bedurften.<a href="#fn8" class="footnote-ref" id="fnref8" role="doc-noteref"><sup>8</sup></a></p>


<table>
<thead>
<tr>
<th>Rang</th>
<th>Property</th>
<th>Anzahl der Verwendung</th>
</tr></thead>
<tbody>
<tr class="odd">
<td>1</td>
<td>P31</td>
<td>7671</td>
</tr>
<tr class="even">
<td>2</td>
<td>P1433</td>
<td>1151</td>
</tr>
<tr class="odd">
<td>3</td>
<td>P407</td>
<td>940</td>
</tr>
<tr class="even">
<td>4</td>
<td>P1476</td>
<td>636</td>
</tr>
<tr class="odd">
<td>5</td>
<td>P577</td>
<td>536</td>
</tr>
<tr class="even">
<td>6</td>
<td>P6216</td>
<td>504</td>
</tr>
<tr class="odd">
<td>7</td>
<td>P921</td>
<td>493</td>
</tr>
<tr class="even">
<td>8</td>
<td>P50</td>
<td>383</td>
</tr>
<tr class="odd">
<td>9</td>
<td>P18</td>
<td>301</td>
</tr>
<tr class="even">
<td>10</td>
<td>P361</td>
<td>137</td>
</tr>
<tr class="odd">
<td>11</td>
<td>P179</td>
<td>99</td>
</tr>
</tbody>
</table>
<div class="caption">
<p>Tabelle 3: Die elf häufigsten Properties mit der Anzahl ihrer Verwendung</p>
</div>
<h2 id="extraktion-der-vorhandenen-metadaten-in-wikisource">Extraktion der vorhandenen Metadaten in Wikisource</h2>
<p>Die für die Anlage bzw. das Update der bibliographischen Wikidata-Items notwendigen Informationen finden sich weitestgehend in der Infobox wie in Abbildung 1 gezeigt. In ihrer Gesamtheit abfragbar sind alle Artikel der Gartenlaube in Wikisource anhand der vergebenen Kategorien. Jeder Artikel ist einer Jahrgangskategorie (Kategorie:Die Gartenlaube (YYYY) Artikel) zugeschrieben und diese wiederum ist eine Unterkategorie der <a href="https://de.wikisource.org/wiki/Kategorie:Die_Gartenlaube_Artikel">Kategorie:Die_Gartenlaube_Artikel</a>.</p>

<p>Zur Extraktion sämtlicher Artikelmetadaten der Gartenlaube wurde ein Python Skript in einem Jupyter Notebook entwickelt (Ablaufplan vgl. Abb. 5) und auf der Mediawiki Jupyter Plattform <a href="https://www.mediawiki.org/wiki/PAWS">PAWS</a> eingesetzt. </p>

<ul>
<li>Das verwendete <a href="../ParseWikiSource_ByCategories__GartenlaubeArtikelParser.ipynb">Skript</a> durchläuft auf Basis der Antwort der Mediawiki-API die Oberkategorie (<a href="https://de.wikisource.org/wiki/Kategorie:Die_Gartenlaube_Artikel">Kategorie:Die_Gartenlaube_Artikel</a>) zur Identifizierung der einzelnen Jahrgänge und holt im weiteren Schritt den Mediawiki-Text jedes einzelnen Artikels ab. </li>
<li><p>Mit RegularExpressions werden die einzelnen Parameter der Textbox auf der Seite extrahiert:</p>
<ul>
<li>Titel</li>
<li>Subtitel</li>
<li>(Erscheinungs-)Jahr</li>
<li>Seite</li>
<li>Heftnummer</li>
<li>Autor</li>
<li>Wikipedia-Link</li>
</ul></li>
<li>Die Werte im Feld Autor können hierbei reine Textstrings sein oder auch Wiki-Links zu den Autorenseiten in der Wikisource. In letzterem Fall ist es daher auch möglich eine Wikidata-ID für einen Autor zu gewinnen. Dafür muss über den vorhandenen Link der Wikisource-Autorenpage eine Mediawiki-API-Abfrage nach der QID gestartet werden.</li>
<li>Der Wikipedia-Link stellt eine inhaltliche Klassifizierung des Artikels dar. Über die Mediawiki-Abfrage lässt sich die QID zur Verwendung als Schlagwort im Wikidata-Item auslesen.</li>
<li>Abschließend werden die je Artikel vorhandenen Daten in einen Satz an <a href="https://tools.wmflabs.org/quickstatements/#/">QuickStatements</a><a href="#fn9" class="footnote-ref" id="fnref9" role="doc-noteref"><sup>9</sup></a>-Befehlen umgewandelt, um den Import der Daten zu ermöglichen.</li>
</ul>

<figure>
<img src="./Pictures/100002010000031A0000046380BA566FD9DA36A0.png" style="width:10.058cm;height:14.573cm" alt="" /><figcaption><br />
Abbildung 5: Ablaufplan des Python-Skripts zur Extraktion der Metadaten in der Wikisource-Vorlage und Konvertierung in QuickStatements-Befehle</figcaption>
</figure>

<h2 id="auswertung-visualisierungen">Auswertung – Visualisierungen</h2>
<p>Die nach Wikidata importierten bibliographischen Daten bilden einen strukturierten und verlinkten Nachweis der in Wikisource transkribierten Zeitschriftenartikel. Dies erlaubt beispielsweise die automatisierte Übernahme der bibliographischen Daten in lokale Bibliothekssysteme oder die Verwendung als Zitationsgrundlage in Literaturdatenbanken für wissenschaftliche Arbeiten.</p>

<p>Gleichzeitig ermöglicht dieser Datenbestand zahlreiche tabellarische Auswertungen und Visualisierungen<a href="#fn10" class="footnote-ref" id="fnref10" role="doc-noteref"><sup>10</sup></a> in Form von Diagrammen oder Karten über die Summe aller Artikel der Zeitschrift:</p>
<ul>
<li>Scholia-Profil der Zeitschrift: <a href="https://tools.wmflabs.org/scholia/venue/Q655617">https://tools.wmflabs.org/scholia/venue/Q655617</a></li>
<li>Tabellarische <a href="https://w.wiki/43s">Abfrage</a> aller Artikel ohne Beschlagwortung und Listen für die Verbesserung der Datenqualität bezüglich bestimmter – unvollständiger – Merkmale (z.B. fehlende Illustratoren oder Untertitel von Illustrationen).</li>
<li><a href="https://query.wikidata.org/embed.html#%23defaultView%3ABubbleChart%0ASELECT%20%20%3FSchlagwort%20%3FSchlagwortLabel%20(COUNT(%3FDie_Gartenlaube)%20AS%20%3Fanzahl)%20WHERE%20%7B%0A%20%20SERVICE%20wikibase%3Alabel%20%7B%20bd%3AserviceParam%20wikibase%3Alanguage%20%22%5BAUTO_LANGUAGE%5D%2Cen%22.%20%7D%0A%20%20%3FDie_Gartenlaube%20wdt%3AP1433%20wd%3AQ655617.%0A%20%20%3FDie_Gartenlaube%20wdt%3AP921%20%3FSchlagwort.%20%0A%0A%7D%0AGROUP%20BY%20%3FSchlagwort%20%3FSchlagwortLabel%0AORDER%20BY%20DESC(%3Fanzahl)">Bubble-Chart</a> für alle Schlagwörter der Artikel, Größe nach ihrer Häufigkeit der Verwendung.</li>
<li><a href="https://w.wiki/BX9">Karte</a> mit allen Orten die in Artikeln beschrieben werden.</li>
</ul>


<h2 id="ausblicke">Ausblick</h2>
<p><a href="https://twitter.com/search?q=%23DieDatenlaube&amp;src=typed_query">#DieDatenlaube</a> versteht sich als ein fortlaufendes Begleitprojekt des Wikisource-Projektes für die Transkription und OCR-Korrektur von <em>Die Gartenlaube</em> der Jahrgänge 1853 bis 1899). Die systematische und strukturierte Beschreibung der Artikel der Zeitschrift in Wikidata war Ausgangspunkt und bleibt weiterhin eine Kernaufgabe, die laufend um neue „Baustellen‟ ergänzt wird wie beispielsweise:</p>
<ul>
<li>Fortführung der inhaltlichen Erschließung der Artikel<a href="#fn11" class="footnote-ref" id="fnref11" role="doc-noteref"><sup>11</sup></a></li>
<li>Identifizierung von fiktionalen Artikeln und generell die Zuordnung zu einer Literaturgattung. (zB Gedichte, Erzählungen aber auch Biographien, Reisebericht etc.)</li>
<li>Verbesserung der Nachweise bei Artikeln die über mehrere Hefte hinweg erschienen sind (aber in Wikisource als [sinnvolle] Einheit dargestellt werden)</li>
<li>Verlinkung der Zusammenhänge von Artikelserien (<em>part of series</em>, <em>has part</em>)</li>
<li>Verlinkung und Verstärkung des Zusammenspiels mit Wikimedia-Commons (zB. Nachweis der biblioraphischen Fundstelle bei Illustrationen)</li>
<li>Entwicklung neuer Fragestellungen für weitere Auswertungen mit Wikidata-Abfragen und Visualisierungen des Datenbestandes</li>
<li>Entwicklung von Open Educational Ressources (OER) auf Basis der Texte und Daten der Illustrierten<a href="#fn12" class="footnote-ref" id="fnref12" role="doc-noteref"><sup>12</sup></a></li>
</ul>
<section class="footnotes" role="doc-endnotes">
<hr />
<ol>
<li id="fn1" role="doc-endnote"><p>Wikimedia Deutschland e.V.: Wikisource-Broschüre, 2019, <a href="https://www.wikidata.org/wiki/Q66818271">(Q66818271)</a>, sowie <a href="https://blog.wikimedia.de/2019/10/16/hilfe-fuer-die-datenlaube-mit-wikisourcewikidata-die-freie-quellensammlung-verbessern/">https://blog.wikimedia.de/2019/10/16/hilfe-fuer-die-datenlaube-mit-wikisourcewikidata-die-freie-quellensammlung-verbessern/</a><a href="#fnref1" class="footnote-back" role="doc-backlink">↩︎</a>.</p></li>
<li id="fn2" role="doc-endnote"><p>Projektstand per 01.11.2019 <a href="https://de.wikisource.org/wiki/Die_Gartenlaube#Projektstand">https://de.wikisource.org/wiki/Die_Gartenlaube#Projektstand</a><a href="#fnref2" class="footnote-back" role="doc-backlink">↩︎</a>.</p></li>
<li id="fn3" role="doc-endnote"><p><a href="https://de.wikisource.org/w/index.php?title=Diskussion:Die_Gartenlaube&amp;oldid=3573624#Vorschlag_für_ein_Basisdatenmodell_der_Artikel_der_Gartenlaube">https://de.wikisource.org/w/index.php?title=Diskussion:Die_Gartenlaube&amp;oldid=3573624#Vorschlag_f%C3%BCr_ein_Basisdatenmodell_der_Artikel_der_Gartenlaube</a> <a href="#fnref3" class="footnote-back" role="doc-backlink">↩︎</a>.</p></li>
<li id="fn4" role="doc-endnote"><p>Fauconnier, Sandra: Structured Data on Commons and GLAM - Wikimania 2019.pdf - Wikimania, 2019. Online: <a href="https://commons.wikimedia.org/wiki/File:Structured_Data_on_Commons_and_GLAM_-_Wikimania_2019.pdf">https://commons.wikimedia.org/wiki/File:Structured_Data_on_Commons_and_GLAM_-_Wikimania_2019.pdf</a>.<a href="#fnref4" class="footnote-back" role="doc-backlink">↩︎</a>.</p></li>
<li id="fn5" role="doc-endnote"><p>SPARQL-Query der entsprechenden Datensätze: <a href="https://w.wiki/Bds">https://w.wiki/Bds</a> <a href="#fnref5" class="footnote-back" role="doc-backlink">↩︎</a>.</p></li>
<li id="fn6" role="doc-endnote"><p>Das „älteste‟ Gartenlaube-Item ist somit <a href="https://www.wikidata.org/w/index.php?title=Q15892076&amp;diff=943569455&amp;oldid=113986055">Q15892076</a>, welches am 05.03.2014 von einem Bot lediglich mit dem Sitelink (keine Labels oder Statements) angelegt wurde. Das jüngste (per 14.11.2019) Item ist <a href="https://www.wikidata.org/w/index.php?title=Q75015200&amp;oldid=1052691032">Q75015200</a><a href="#fnref6" class="footnote-back" role="doc-backlink">↩︎</a>.</p></li>
<li id="fn7" role="doc-endnote"><p>Alle Berechnungen und Auswertungen finden sich im Jupyter-Notebook <a href="../Analyzing_WikidataItems.ipynb">Analyzing_WikidataItems.ipynb</a><a href="#fnref7" class="footnote-back" role="doc-backlink">↩︎</a>.</p></li>
<li id="fn8" role="doc-endnote"><p>Anzumerken sei noch, dass diese numerische Auswertung letztlich auch dem Umstand geschuldet ist, dass zum damaligen Zeitpunkt die Analyse von Items mittels ShapeExpressions noch nicht in Wikidata derart umgesetzt war, wie es zum gegenwärtigen Zeitpunkt mit Verwendung von EntitySchema möglich ist.<a href="#fnref8" class="footnote-back" role="doc-backlink">↩︎</a>.</p></li>
<li id="fn9" role="doc-endnote"><p>QuickStatements wurde als Tool verwendet, da OpenRefine zwar für die Bearbeitung der großen Masse an Daten gewisse Vorteile und eine tabellarische Übersichtlichkeit gebracht hätte, allerdings ist das Anlegen neuer Items mit Sitelinks in ein Wikiprojekt mit OpenRefine (noch) nicht möglich. <a href="#fnref9" class="footnote-back" role="doc-backlink">↩︎</a>.</p></li>
<li id="fn10" role="doc-endnote"><p>Unterschiedliche Auswertungen und Visualisierungen werden auf der Wikisource Diskussionsseite der Gartenlaube dokumentiert: <a href="https://de.wikisource.org/wiki/Wikisource:Wikidata#Die_Gartenlaube_(Abfragen">https://de.wikisource.org/wiki/Wikisource:Wikidata#Die_Gartenlaube_(Abfragen</a>).<a href="#fnref10" class="footnote-back" role="doc-backlink">↩︎</a>.</p></li>
<li id="fn11" role="doc-endnote"><p>Bemme, Jens: Hilfe für die Datenlaube: mit [[Wikisource+Wikidata]] die freie Quellensammlung verbessern. (2019) <a href="https://blog.wikimedia.de/2019/10/16/hilfe-fuer-die-datenlaube-mit-wikisourcewikidata-die-freie-quellensammlung-verbessern/">https://blog.wikimedia.de/2019/10/16/hilfe-fuer-die-datenlaube-mit-wikisourcewikidata-die-freie-quellensammlung-verbessern/</a> <a href="#fnref11" class="footnote-back" role="doc-backlink">↩︎</a>.</p></li>
<li id="fn12" role="doc-endnote"><p>Vgl. HistoDigitaLE an der Universität Leipzig, <a href="https://de.wikisource.org/wiki/Wikisource:OER">https://de.wikisource.org/wiki/Wikisource:OER</a><a href="#fnref12" class="footnote-back" role="doc-backlink">↩︎</a>.</p></li>
</ol>
</section>

<p>Veröffentlicht: 18.11.2019, <a href="https://www.wikidata.org/wiki/Q75682119">(Q75682119)</a></p>
<img alt="Creative Commons Lizenzvertrag" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/80x15.png" />&nbsp;&nbsp;&nbsp;<a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Namensnennung 4.0 International Lizenz</a> <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">
