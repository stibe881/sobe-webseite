# Offene Stellen

Die offenen Stellen stehen nicht im Repository. Sie kommen von der
Stellenbörse **publicjobs.ch**, auf der sie ohnehin ausgeschrieben werden.
Eine zweite, von Hand gepflegte Liste lief immer hinterher – und eine
Ausschreibung, die längst besetzt ist, kostet jemanden eine Bewerbung.

## Was eingestellt ist

In `inhalt/einstellungen.json`, Abschnitt `stellenboerse` – auch in der
Redaktion unter *Einstellungen* zu ändern:

| Schlüssel | Bedeutung |
| --- | --- |
| `kdNr` | die Kundennummer bei publicjobs (zurzeit `108447`) |
| `quelle` | die Adresse der Schnittstelle |
| `limit` | wie viele Stellen höchstens geholt werden |

## Warum nicht das Widget von publicjobs

publicjobs gibt einen fertigen Schnipsel heraus, der ein Skript nachlädt
und die Liste selbst zeichnet. Dagegen sprachen drei Dinge:

- Das Skript ruft `jQuery.each` auf, **bringt jQuery aber nicht mit**. Die
  Seite hat kein jQuery, also wären es zwei Nachladungen statt einer –
  rund 120 Kilobyte für acht Zeilen Text.
- Es legt eine eigene Schriftart und ein eigenes Raster über die Kachel.
  Die Liste sähe aus wie ein Fremdkörper.
- Der Dienst gibt dieselben Daten als JSON heraus, mit
  `Access-Control-Allow-Origin: *`. Das kann der Browser direkt lesen.

Geholt wird also nur die Antwort, gezeichnet wird in den Zeilen dieser
Seite (`vorlagen/seite.js`, Abschnitt 2). Der Preis dafür: Die
Schnittstelle ist nicht als öffentliche API dokumentiert. Ändert
publicjobs sie, bleibt die Liste leer und es erscheint der Hinweis mit der
E-Mail-Adresse – die Seite wirkt nie kaputt, aber jemand muss es merken.

## Was dabei zu beachten ist

- **Fremde Daten sind Text, nie Markup.** Titel kommen mit HTML-Entitäten
  («Heilp&auml;dagog*in») und werden über ein losgelöstes `<textarea>`
  entschlüsselt; eingesetzt wird mit `textContent`. Adressen werden über
  `new URL()` geprüft und nur als Verweis gesetzt, wenn `http:` oder
  `https:` davorsteht – `javascript:` ergibt keinen Verweis.
- **Suchmaschinen sehen die Liste nicht.** Sie entsteht erst im Browser.
  Wer die Stellen auffindbar will, verlinkt sie bei publicjobs.
- **Der Ortsfilter** entsteht aus den Orten der Antwort (Baar, Menzingen)
  und erscheint erst ab zwei verschiedenen Orten.
- **Die Sprache** wird mitgeschickt, ändert aber wenig: Nur Französisch
  hat eine übersetzte Überschrift, die Stellentitel bleiben deutsch.

## Die alten Stellenseiten

Unter `quelle/stellen/` liegen noch acht Ausschreibungen aus der
WordPress-Übernahme. Sie werden weiterhin zu Seiten gebaut
(`/jobs/<kennung>/`), aber von der Übersicht **nicht mehr verlinkt** – die
zeigt jetzt die Börse. Sie stehen damit noch in der Sitemap und in der
Suche dieser Seite, und ihr Inhalt ist der Stand der Übernahme. Wer sie
nicht mehr will, löscht die Dateien in `quelle/stellen/` und baut neu.
