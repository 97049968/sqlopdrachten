# Sql basis

### Database 
Maak gebruik van het sql-bestand reisbureau.sql en voer hierop onderstaande opdrachten uit.
Theorie kun je vinden op: https://www.edutorial.nl/dbq/introductie/

### Queries reisbureau
* Geef de namen van de klanten die in Rotterdam wonen 
SELECT `Naam`
FROM klanten
WHERE `Plaats` = 'Rotterdam';


* Geef de namen van de klanten die geen reis geboekt hebben.
SELECT klanten.naam
FROM klanten
LEFT JOIN boekingen
    ON klanten.`Klantnummer` = boekingen.`Klantnummer`
WHERE boekingen.`Klantnummer` IS NULL;


* Hoeveel klanten komen er niet uit Rotterdam
SELECT `Naam`
FROM klanten
WHERE `Plaats` <> 'Rotterdam';


* Hoeveel reizen hebben als bestemming Afrika?
SELECT COUNT(*)
FROM bestemmingen
WHERE `Werelddeel` = 'Afrika';


* Hoeveel moeten de klanten, die naar Azië gaan, in totaal betalen?
SELECT SUM(reizen.`Prijs per persoon` * 
           (COALESCE(boekingen.`Aantal volwassenen`,0) + COALESCE(boekingen.`Aantal kinderen`,0))
          ) AS totaal_bedrag
FROM boekingen
JOIN reizen
    ON boekingen.Reisnummer = reizen.Reisnummer
JOIN bestemmingen
    ON reizen.Bestemmingcode = bestemmingen.Bestemmingcode
WHERE bestemmingen.Werelddeel = 'Azie';


* Geef de namen van de klanten die met kinderen op reis gaan.
SELECT DISTINCT klanten.naam
FROM klanten klanten
JOIN boekingen boekingen
    ON klanten.klantnummer = boekingen.klantnummer
WHERE boekingen.`Aantal kinderen` > 0;


* Hoeveel verschillende reizen kun je boeken bij dit reisbureau?
SELECT COUNT(*)
FROM reizen;


* Geef de naam en postcode van de klanten die in een postcode gebied wonen dat begint met een 9.
SELECT naam, postcode
FROM klanten
WHERE postcode LIKE '9%';


* Groepeer de klanten op woonplaats. Geef de woonplaats en het aantal klanten per woonplaats.
SELECT `Plaats`, COUNT(*) AS aantal_klanten
FROM klanten
GROUP BY `Plaats`;


* Geef naam en datum van de klanten die voor de maand April een reis hebbben geboekt.
SELECT klanten.naam, boekingen.Boekdatum
FROM boekingen
JOIN klanten
    ON klanten.Klantnummer = boekingen.Klantnummer
WHERE MONTH(boekingen.Boekdatum) = 4;

* Geef de namen van klanten, het werelddeel van de bestemming en het aantal dagen van de reis voor boekingen van minimaal 15 dagen.
SELECT klanten.Klantnummer,
       bestemmingen.Werelddeel,
       reizen.`Aantal dagen`
FROM boekingen
JOIN reizen
    ON reizen.Reisnummer = boekingen.Reisnummer
JOIN bestemmingen
    ON bestemmingen.Bestemmingcode = reizen.Bestemmingcode
JOIN klanten
    ON klanten.Klantnummer = boekingen.Klantnummer
WHERE reizen.`Aantal dagen` >= 15;
