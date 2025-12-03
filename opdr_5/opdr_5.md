# Sql basis

### Database
Maak gebruik van het sql-bestand flits.sql en voer hierop onderstaande opdrachten uit.
Theorie kun je vinden op: https://www.edutorial.nl/dbq/introductie/

### Queries flitspaal
* Welke cameras zijn er en waar staan ze (id, address, city, max_speed).
SELECT id, address, city, max_speed FROM cameras

* Overzicht van boetes op 50km wegen
SELECT *
FROM fines
WHERE speed_limit = 50;

#en
SELECT *
FROM fines
WHERE speed_limit = 50
  AND speed_excess > 0;


* Overzicht van overtredingen van 1 kenteken.
SELECT license
FROM flashes
WHERE speed > 0
LIMIT 1;


* N.a.w.(naam, adress, woonplaats) -gegevens van de hardrijders die het meest in de fout zijn gegaan. (top 10)
SELECT 
    licenses.first_name,
    licenses.address,
    licenses.city,
    COUNT(fines.id) AS aantal_overtredingen
FROM licenses
JOIN flashes ON licenses.id = flashes.license
JOIN fines ON flashes.id = fines.id
WHERE fines.speed_excess > 2
GROUP BY licenses.id
ORDER BY aantal_overtredingen DESC
LIMIT 10;


* Welke camera’s (id, address, city) meten de meeste snelheidsovertredingen (top 10)
SELECT 
    cameras.id,
    cameras.address,
    cameras.city,
    COUNT(fines.id) AS aantal_overtredingen
FROM cameras
JOIN flashes ON cameras.id = flashes.camera_id
JOIN fines ON flashes.id = fines.id
WHERE fines.speed_excess > 2
GROUP BY cameras.id
ORDER BY aantal_overtredingen DESC
LIMIT 10;


* Welke auto’s (kenteken, merk, type) zijn het meeste geflitst
SELECT 
    flashes.license AS kenteken,
    COUNT(fines.id) AS aantal_flitsen
FROM flashes
JOIN fines ON flashes.id = fines.id
WHERE fines.speed_excess > 2
GROUP BY flashes.license
ORDER BY aantal_flitsen DESC
LIMIT 10;


* Welke flitspaal (=camera met id, address, city) flitst het meest (top 10)
SELECT 
    cameras.id,
    cameras.address,
    cameras.city,
    COUNT(fines.id) AS aantal_flitsen
FROM cameras
JOIN flashes ON cameras.id = flashes.camera_id
JOIN fines ON flashes.id = fines.id
WHERE fines.speed_excess > 2
GROUP BY cameras.id
ORDER BY aantal_flitsen DESC
LIMIT 10;


* Kentekens van auto’s die het hoogste bedrag aan boetes hebben gekregen (top 10)
SELECT 
    licenses.license,
    COUNT(fines.id) AS aantal_flitsen
FROM licenses
JOIN flashes ON licenses.id = flashes.license
JOIN fines ON flashes.id = fines.id
WHERE fines.speed_excess > 2
GROUP BY licenses.id
ORDER BY aantal_flitsen DESC
LIMIT 10;


* Overzicht van auto’s (kenteken, merk, type) waarvan het kenteken overeenkomt met sitecode 10 (zoals X-999-XX) https://nl.wikipedia.org/wiki/Nederlands_kenteken.
SELECT 
    flashes.license AS kenteken
FROM flashes
WHERE flashes.license REGEXP '^[A-Z]-[0-9]{3}-[A-Z]{2}$';
