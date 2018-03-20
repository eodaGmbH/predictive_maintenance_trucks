---
title: "Luftdrucksystemfehler bei Scania Trucks"
output: 
  flexdashboard::flex_dashboard:
    css: styles.css
    orientation: columns
    vertical_layout: fill
---






Datensatz & Problematik
=======================================================================

Column {data-width=1}
-----------------------------------------------------------------------

### Problemstellung
Der Scania-Trucks Datensatz kommt mit 171 anonymisierten Featuren sowie 76000 Observationen, welche im Verhältnis 60.000 zu 16.000 in Trainings- bzw. Testdaten aufgeteilt sind. Jede Observation enthält die Information, ob ein Fehler im Luftdruckssystem ( im Folgenden LDS genannt ) vorliegt oder nicht. <br>
Die Problemstellung lautet: <br>
<div class = "bordered"><i>
Sage für jede Observation im Testdatensatz voraus, ob ein Fehler im LDS vorliegt oder nicht. Minimiere dabei folgende Kostenfunktion: <br>
<b>$cost(FP, FN) = 10 \cdot FP + 500 \cdot FN$</b> <br>
Es handelt sich hierbei also um ein <b>Klassifizierungsproblem</b>, mit der Kostenfunktion $cost(FP, FN)$ als Metrik. <br>
</i></div> <br>
<b><u>Fehler 1. Art (FP):</u></b> <br>
Der Fehler 1. Art, hier <b>F</b>alse <b>P</b>ositive, bedeutet, dass man einer Observation einen Fehler im LDS zuschreiben, obwohl das Fahrzeug fehlerfrei ist. Man schickt das Fahrzeug also in die Werkstatt, obwohl kein Fehler vorhanden ist. <br><br>
<b><u>Fehler 2. Art (FN):</u></b> <br>
Der Fehler 2. Art, hier <b>F</b>alse <b>N</b>egative, bedeutet, dass man eine Observation als fehlerfrei einstuft, obwohl das Fahrezeug einen Fehler im LDS aufweist. <br><br>
<div class = "bordered"><i>
Der Fehler 1. Art ist logischerweise deutlich kostengünstiger gewichtet als der Fehler 2. Art. Es ist wesentlich günstiger einen LKW fälschlicherweise in die Werkstatt zur Überprüfung zu schicken, als dass ein LKW, bedingt durch einen LDS-Fehler, auf der Strecke liegen bleibt.
</i></div> <br><br>

Column {data-width=3}
----------------------------------------------------------------------

### Überblick Datensatz
Aus der Beschreibung des Scnaia-Trucks Problems lässt sich herauslesen, dass gewisse Feature Histogrammdaten abbilden. Das heißt, dass ein Feature auf mehrere Container aufgeteilt ist, wobei der Wert im Container $i$ die Aufenthaltsdauer in der Klasse $A_i$ bedeutet, wobei $A$ das obergeordnete Feature ist. Ein Beispiel für ein solches <b>Histogrammfeature</b> findet sich schon recht zu Beginn des Datensatzes: <br>

<table class="table table-striped" style="margin-left: auto; margin-right: auto;">
<caption>Histogrammdaten</caption>
 <thead><tr>
<th style="text-align:right;"> ag_000 </th>
   <th style="text-align:right;"> ag_001 </th>
   <th style="text-align:right;"> ag_002 </th>
   <th style="text-align:right;"> ag_003 </th>
   <th style="text-align:right;"> ag_004 </th>
   <th style="text-align:right;"> ag_005 </th>
   <th style="text-align:right;"> ag_006 </th>
   <th style="text-align:right;"> ag_007 </th>
   <th style="text-align:right;"> ag_008 </th>
   <th style="text-align:right;"> ag_009 </th>
  </tr></thead>
<tbody>
<tr>
<td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 37250 </td>
   <td style="text-align:right;"> 1432864 </td>
   <td style="text-align:right;"> 3664156 </td>
   <td style="text-align:right;"> 1007684 </td>
   <td style="text-align:right;"> 25896 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
<tr>
<td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 18254 </td>
   <td style="text-align:right;"> 653294 </td>
   <td style="text-align:right;"> 1720800 </td>
   <td style="text-align:right;"> 516724 </td>
   <td style="text-align:right;"> 31642 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
<tr>
<td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1648 </td>
   <td style="text-align:right;"> 370592 </td>
   <td style="text-align:right;"> 1883374 </td>
   <td style="text-align:right;"> 292936 </td>
   <td style="text-align:right;"> 12016 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
<tr>
<td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 318 </td>
   <td style="text-align:right;"> 2212 </td>
   <td style="text-align:right;"> 3232 </td>
   <td style="text-align:right;"> 1872 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
<tr>
<td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 43752 </td>
   <td style="text-align:right;"> 1966618 </td>
   <td style="text-align:right;"> 1800340 </td>
   <td style="text-align:right;"> 131646 </td>
   <td style="text-align:right;"> 4588 </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
</tbody>
</table>







