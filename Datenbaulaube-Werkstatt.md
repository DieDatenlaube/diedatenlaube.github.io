<h1>Neues aus der DatenBauLaube - ein Werkstatt-Zwischenbericht</h1>
Im Nachfolgenden möchte ich kurz skizzieren, was man mit den tollen "Die Gartenlaube"-<a href="https://doi.org/10.5281/zenodo.5787665">Wikisource-Dumps</a> in JSON so anfangen könnte, zunächst erstmal für eine schnöde Textanalyse, mit derer Hilfe aber neue Forschungsfragen und Tutorials entstehen (können), z.B. was cooles mit dem <a href="https://www.nltk.org/">Natural Language Toolkit</a>.

<ol>
<li><b>Datei(en) downloaden</b>:<br/>
Mit einem wget-Befehl auf einer Console ein File von zenodo.org downloaden und mit der -O-Option einen lokalen Namen festlegen:<br/>
  <code>wget -O gartenlaube_1891.json https://zenodo.org/record/5787665/files/GartenlaubeSeitenText_Kategorie:Die%20Gartenlaube%20%281891%29_1639446194.json?download=1</code>
<li><b>mit PHP die Inhalte umkopieren</b>:<br/>
Die Idee ist, sämtliche Textinhalte in ihre einzelnen Wörter zerlegen (explode jeweils an einem Leerzeichen) und SQL-Statement in ein Textfile zu schreiben (fopen ... explode ... foreach ... "insert into"-fwrite)
 <pre>
$filename="insert_words.sql";
    if (!$fp = fopen($filename, "a")) {
         print "Kann die Datei $filename nicht öffnen";
         exit;
    }

if ($file = fopen("gartenlaube_1891.json", "r")) {
    while(!feof($file)) {
        $line = fgets($file);
        # do same stuff with the $line
        $linearray = explode(" ",  $line);
        foreach( $linearray as $word){
        fwrite($fp, "insert into words_bulk (word) values ('".addslashes(strip_tags($word))."');\n");
        }
    }
    fclose($file);
}
fclose($fp);
</pre>
Nachdem das Skript gegen die Datenbank gelaufen lassen worden ist, erfolgten erste Bereinigungen, die immer noch mit dem im Sourcefile enthaltenen HTML-Code zusammenhängen.
<li><b>Some SQL magic: </b>bulk_words to words_table<br/>
In der MySQL-Datenbank können wiederum mit dem tee-Befehl selbst neue SQL-Statements erzeugt werden, die vorher per SQL automatisch sortiert und gruppiert wurden (und mit count(*) auch noch zählbar sind):
  <code>SELECT concat('insert into words (word) values (''',word,''');') FROM words_bulk GROUP BY word;</code>
<li><b>analyzing SELECT UPDATE DELETE</b>:<br/>
Als erstes hatte mich interessiert, ob es noch besondere Substantive gibt, die als neue Main-Subjects herhalten könnten:
  <code>UPDATE words SET typ='noun' WHERE word REGEXP BINARY '[A-Z]';</code> - Diese "noun" bin ich dann einfach durchgegangen: 
<li><b>next steps</b>:<br/>
Kürzlich hatte ich ein kurzweiliges <a href="https://www.slub-dresden.de/besuchen/veranstaltungen/details/veranstaltung/34177">NLTK-Tutorial</a> und dort kamen Stopwort-Dictionaries und Konkordanzlisten zur Sprache - da wird es bald eine "DieGartenlaube"-Edition dazu geben...
</ol>

<p>Matthias Erfurth, 28. Dezember 2021</p>
<img alt="Creative Commons Lizenzvertrag" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/80x15.png" />&nbsp;&nbsp;&nbsp;<a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Namensnennung 4.0 International Lizenz</a>
