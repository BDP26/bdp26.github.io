---

layout: post
title: "GridWatch"
author: Natalie Jakab, Sarruja Sabesan und Gwendoline Vocat

---

<img src="../picture/ZHAW_LOGO_2.webp" alt="ZHAW Logo" width="150">

*Blogartikel GridWatch*  
*ZHAW Data Science | PM4 - Big Data Projekt | Mai 2026*

---

# Grün genug? Wann erneuerbare Energie die Schweiz wirklich versorgt


Jeden Abend, wenn die Sonne untergeht und der Strombedarf steigt, stellt sich eine unbequeme Frage: Woher kommt der Strom gerade und reicht er? Die Schweiz will bis 2050 CO₂-neutral sein. Erneuerbare Energiequellen sollen den Löwenanteil übernehmen. Doch wie weit ist die Schweiz heute wirklich?

Im Projekt **GridWatch** haben wir vier Jahre Schweizer Stromdaten zusammengeführt und analysiert: stündliche Produktions- und Verbrauchsdaten, grenzüberschreitende Import- und Exportflüsse zu allen vier Nachbarländern, und Wetterdaten aus 61 Messstationen quer durch die Schweiz. Das Bild, das sich zeigt, ist präzise und lehrreich.

---

## 2050 ist näher als gedacht

Drei Dinge haben uns dazu bewogen, genauer hinzuschauen.

**Erstens: Energiewende-Druck.** Die Schweiz will bis 2050 CO₂-neutral sein. Erneuerbare Quellen decken den Bedarf noch nicht gleichmässig über Tag und Jahr.

**Zweitens: Fehlende Datentransparenz.** Produktion, Verbrauch und Wetterdaten liegen nicht gemeinsam auf einer zentralen Plattform vor. Wir haben sie zusammengeführt.

**Drittens: Importabhängigkeit.** Besonders im Winter ist die Schweiz auf Strom aus dem Ausland angewiesen, in unseren Daten zeigen sich Importanteile von bis zu 30 %.

---


## Was die Daten wirklich zeigen

Unser Projekt war von drei konkreten Forschungsfragen geleitet. Hier die Kurzfassungen:

**F1: Wann übersteigt die erneuerbare Produktion den Verbrauch?**  
Im Sommer, zwischen 14 und 17 Uhr, decken Erneuerbare bis zu 58% des Schweizer Strombedarfs, eine vollständige Selbstversorgung bleibt jedoch aus. Die Lücke schliesst die Schweiz durch Kernkraft und Importe.

**F2: Wie verändert sich der Import-/Exportbedarf, wenn die Schweiz Solar- oder Windkapazität massiv ausbaut?**  
Solar dreht die Bilanz im Sommer und Wind glättet sie ganzjährig. Was das konkret bedeutet, zeigen die Szenarien unten.

**F3: Wie hat sich das Verhältnis Erneuerbar/Verbrauch über die Jahre entwickelt?**  
Der Trend zeigt nach oben. Solar wächst jedes Jahr messbar. Aber der Fortschritt reicht bisher nicht aus, um das strukturelle Winterdefizit zu schliessen.

---

## Aus drei Quellen wird ein Bild

Die eigentliche Herausforderung war die Infrastruktur: Wir kombinierten stündliche Stromdaten von **ENTSO-E**, Produktionsdaten von **Swissgrid** und Wetterdaten von **Open-Meteo** in einer **TimescaleDB**. Ein Scheduler aktualisiert die Daten täglich; Qualitätschecks erkennen Lücken und Zeitumstellungsfehler.

**Saubere Daten entstehen nicht von selbst.**

![Architekturdiagramm](../picture/pipeline.png)
> 📷 *Architekturdiagramm: Datenpipeline*

---

## Das Winterproblem: wenig Sonne, viel Verbrauch
Das ist das klassische Schweizer Winterproblem: **Maximaler Bedarf** trifft auf **minimale erneuerbare Produktion**.
 Steigt die Temperatur (gelb), sinkt der Verbrauch (grün) und umgekehrt.

![Verbrauch_vs_Temperatur](../picture/Verbrauch_vs_Temperatur.png)
> 📷 *Verbrauch vs Temperatur*


Solarproduktion und Sonnenstrahlung verlaufen saisonal: hoch im Sommer, tief im Winter. Wind ist weniger saisonal und schwankt unregelmässiger über das ganze Jahr.

<table style="border: none; border-collapse: collapse; width: 100%; table-layout: fixed;">
  <tr>
    <td style="border: none; width: 100%; text-align: center; padding-right: 20px;">
      <img src="../picture/Solar-Produktion_vs_Sonnenstrahlung.png" width="150%">
      <br>📷 <em>Solar-Produktion vs Sonnenstrahlung</em>
    </td>
    <td style="border: none; width: 50%; text-align: center; padding-left: 50px;">
      <img src="../picture/Wind-Produktion_vs_Windgeschwindigkeit.png" width="100%">
      <br>📷 <em>Wind-Produktion vs Windgeschwindigkeit</em>
    </td>
  </tr>
</table>

---


## Solar oder Wind: Wer rettet den Winter?

Für die zweite Forschungsfrage haben wir Szenarien modelliert: Was passiert, wenn Solar- und Windkapazitäten bis zum Zehnfachen des heutigen Ausbaus skaliert werden?


<table style="border: none; border-collapse: collapse; width: 100%;">
  <tr>
    <td style="border: none; width: 50%; text-align: center; padding-right: 10px;">
      <img src="../picture/Szenario_Solar.png" width="100%">
      <br>📷 <em>Solar Szenario</em>
    </td>
    <td style="border: none; width: 50%; text-align: center; padding-left: 10px;">
      <img src="../picture/Szenario_Wind.png" width="100%">
      <br>📷 <em>Wind Szenario</em>
    </td>
  </tr>
</table>


> Solar ×10 eliminiert den Sommerimport während der Sonnenstunden, führt aber zu massiver lokaler Überproduktion. Nicht empfohlen.  
> Wind reduziert den Import ganzjährig gleichmässiger, ohne saisonale Spitzen. Jedoch wäre ein massiver Ausbau nötig.

Diese Daten legen nahe: Energiespeicher werden zur Schlüsseltechnologie. Wind ist saisonal ausgeglichener, steht aber vor Standort-, Bewilligungs- und Akzeptanzfragen.

---

## Was wir auf dem Weg gelernt haben
 
Datenprojekte lehren am meisten durch das, was schiefläuft. **Überlastete Schnittstellen** zwangen uns zu langsameren, dafür zuverlässigeren Anfragen. Die **Zeitumstellung** auf Sommer- und Winterzeit erzeugt Stunden, die doppelt vorkommen oder ganz fehlen, ein echtes Datenproblem hinter einem scheinbaren Detail. Und **Datenlücken**, die sich nicht automatisch füllen lassen, muss man irgendwann akzeptieren und weitermachen.
 
Kurz: **Datenqualität entscheidet. Saubere Pipelines sind keine Selbstverständlichkeit.**
 
---
 

## Erneuerbar, aber nicht rund um die Uhr!

Die Antwort auf die Titelfrage lautet: **Sonne, Wind und Wasser decken den Schweizer Strombedarf, aber nur im Sommer, und nur tagsüber.**

> Ohne Speichertechnologie bleibt die erneuerbare Vollversorgung eine Sommerlösung. Die eigentliche Frage für die nächsten Jahre ist nicht, ob die Schweiz mehr Solar baut, sondern wohin der überschüssige Mittagsstrom fliesst.

**Die Energiewende ist kein Sprint!** Sie ist eine jahrzehntelange Aufgabe. Mit den richtigen Daten, einer soliden Pipeline und einem guten Dashboard sieht man den aktuellen Stand.

---

*Habt ihr selbst Erfahrungen mit Solaranlagen oder Speicherlösungen? Teilt eure Erfahrungen in den Kommentaren*

*Datenquellen: [ENTSO-E](https://transparency.entsoe.eu/), [Swissgrid](https://www.swissgrid.ch/), [Open-Meteo](https://open-meteo.com/). Analysezeitraum: 2022–2026. Code: https://github.com/BDP26/pm4-GridWatch/.*
