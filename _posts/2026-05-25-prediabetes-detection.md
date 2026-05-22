---
layout: post
title: "Dem Diabetes-Risiko auf der Spur: Wie deine Garmin-Uhr zum Lebensretter werden könnte"
author: Abishanth Jeyatheeswaran, Alex Smolders und Thierry Zürcher
---

Wearables wie die **Garmin Vivosmart 5** gehören heute für viele Menschen zum Alltag. Sie messen Herzfrequenz, Schlaf, Aktivität und Stress rund um die Uhr. Doch könnten solche Geräte in Zukunft noch mehr leisten als Fitness-Tracking? Genau dieser Frage widmete sich unsere wissenschaftliche Arbeit. Ziel war es zu untersuchen, ob Daten aus kommerziellen Smartwatches und medizinischen Sensoren genutzt werden können, um Prädiabetes frühzeitig zu erkennen und Blutzuckerwerte vorherzusagen.

## Das unsichtbare Risiko Prädiabetes

Prädiabetes beschreibt einen Zustand, bei dem der Blutzuckerspiegel bereits erhöht ist, ohne jedoch die Grenzwerte eines *Diabetes Typ 2* zu erreichen. Das Problematische daran ist, dass Betroffene häufig keine Symptome bemerken. Viele Menschen wissen deshalb jahrelang nichts von ihrem Risiko.

Dabei wäre gerade dieses Stadium oft noch umkehrbar. Durch mehr Bewegung, eine gesündere Ernährung und Veränderungen im Lebensstil kann die Entwicklung zu einer chronischen Erkrankung häufig verhindert werden. Die Diagnose erfolgt heute jedoch meist über Bluttests in der Arztpraxis. Eine kontinuierliche und schmerzfreie Überwachung im Alltag existiert bislang kaum.

Hier setzt das Projekt an: Können intelligente Algorithmen aus alltäglichen Sensordaten erkennen, ob sich im Körper eine Stoffwechselstörung entwickelt?

## Zwei Forschungsansätze

Die Untersuchung bestand aus zwei komplementären Forschungsbereichen:

* **FF1 – Prädiabetes-Screening:** Kann eine gewöhnliche Smartwatch ein zuverlässiges Prädiabetes-Screening im Alltag ermöglichen?
* **FF2 – Blutzuckervorhersage:** Lassen sich zukünftige Blutzuckerwerte aus physiologischen Daten vorhersagen?

Für das erste Teilprojekt wurden Daten von 61 Testpersonen aus dem *AI-READI-Datensatz* analysiert. Ziel war es herauszufinden, ob Sensordaten ausreichen, um Personen mit erhöhtem Risiko zuverlässig zu erkennen.

## Wie die KI die Sensordaten analysiert

Für die Analyse wurde ein KI-Modell namens **LightGBM** eingesetzt. Dieses Modell eignet sich besonders gut für unvollständige und komplexe Alltagsdaten. Analysiert wurden unter anderem:

* Herzfrequenz und Atemfrequenz
* Schlafphasen und Stresslevel
* Bewegungs- und Aktivitätsdaten

Das Modell erreichte zunächst eine Genauigkeit von **68.9 %**. Bei genauerer Analyse zeigte sich jedoch, dass vor allem das Alter der Testpersonen entscheidend war. Ältere Menschen haben statistisch ein höheres Risiko für Prädiabetes. Entfernte man das Alter aus dem Modell und nutzte nur die reinen Smartwatch-Daten, sank die Genauigkeit deutlich auf **47.5 %**.

Die Ergebnisse zeigen, dass aktuelle Wearables im Alltag noch mit mehreren Problemen kämpfen:

* Datenlücken durch unregelmässiges Tragen der Geräte
* Fehlende Kontinuität bei den Messungen
* Begrenzte Aussagekraft einzelner Sensorwerte

![Abbildung 1: Daten-Pipeline und Feature-Importance der AI-READI-Analyse](../assets/img/ff1_ai_readi_prediabetes.png)

## Der biologische Rhythmus als Schlüssel

Deutlich erfolgreicher war der zweite Forschungsansatz. Hier ging es darum, zukünftige Blutzuckerwerte über einen Zeitraum von 30 Minuten vorherzusagen. Dafür wurde ein **Quantile-XGBoost-Modell** entwickelt.

Der entscheidende Fortschritt lag in der Integration sogenannter *zirkadianer Merkmale*. Der menschliche Stoffwechsel folgt einem biologischen Tagesrhythmus. Durch die Einbindung der Tageszeit konnte das Modell deutlich präzisere Vorhersagen treffen.

Besonders wichtig waren dabei:

* die durchschnittliche Herzfrequenz
* zeitliche Muster im Tagesverlauf
* physiologische Veränderungen über längere Zeiträume

Das Modell erreichte einen mittleren prozentualen Fehler (*MAPE*) von nur **9.14 %**. Die Vorhersagen lagen damit durchschnittlich weniger als zehn Prozent vom tatsächlich gemessenen Blutzuckerwert entfernt.

## Klinische Sicherheit der Vorhersagen

In der Medizin reicht eine gute statistische Genauigkeit alleine nicht aus. Fehlerhafte Vorhersagen könnten falsche therapeutische Entscheidungen auslösen. Deshalb wurde zusätzlich die sogenannte **Clarke Error Grid Analysis** verwendet.

Dieses Verfahren bewertet, ob Vorhersagen klinisch sicher sind. Die Ergebnisse waren vielversprechend:

* **99.7 %** aller Vorhersagen lagen in den sicheren Zonen A und B
* Nur **0.3 %** wichen stärker ab
* Keine Vorhersage führte zu einer gefährlichen Fehleinschätzung

![Abbildung 2: Clarke Error Grid Analysis — Verteilung in den Zonen A und B](../assets/img/clarke_diagramm.png)

## Ausblick

Unser Vorhersagemodell dient insgesamt als überzeugender **Proof of Concept**. Die Ergebnisse zeigen, dass der gewählte Ansatz grundsätzlich funktioniert und eine solide Grundlage für zukünftige Entwicklungen bietet.

Für eine höhere Genauigkeit wären künftig jedoch erweiterte Modelle notwendig, welche komplexere Zusammenhänge zwischen den verschiedenen Einflussfaktoren besser berücksichtigen. Aktuelle Forschungsergebnisse bestätigen, dass solche tieferen Datenstrukturen entscheidend für präzisere medizinische Vorhersagen sind.

Obwohl eine Garmin-Uhr heute noch keine ärztliche Diagnose ersetzen kann, zeigt das Projekt das Potenzial moderner Wearables in der Medizin. In Zukunft könnten uns solche Geräte frühzeitig warnen, wenn sich unsere Stoffwechselwerte über längere Zeit verschlechtern – noch bevor erste Symptome auftreten.
