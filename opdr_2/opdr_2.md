# Sql basis

### Database gegevens aanpassen
Maak gebruik van het sql-bestand reisbureau.sql en voer hierop onderstaande opdrachten uit.
Theorie kun je vinden op: https://www.edutorial.nl/dbq/introductie/

### Opdracht 1
* Geef de query om alle tabellen in de database 'reisbureau' weer te gegeven
USE reisbureau;
SHOW TABLES

### Opdracht 2
* Voeg 2 nieuwe klanten toe aan de tabel 'customers' (je mag de waarden zelf bedenken)
#klant 1
INSERT INTO customers (`first_name`, `last_name`, `email`, `country_code`) 
VALUES ('Ijzer', 'Sterk', 'Ijzer.s@gmail.com', 'da_DK')

#klant 2
INSERT INTO customers (`first_name`, `last_name`, `email`, `country_code`) 
VALUES ('Valor', 'Antiek', 'V.antiek@gmail.com', 'da_DK')

#controleren
SELECT * FROM customers 
WHERE `first_name` IN ('Ijzer', 'Valor')

### Opdracht 3
* Geef de query om de eerste 10 boekingen te verwijderen (reservations)
DELETE FROM reservations
ORDER BY id ASC
LIMIT 10;

### Opdracht 4
* De klant met id 13 is verhuist naar 'De van der veldensteeg 81' in 'Apeldoorn'.
* Pas het record aan en geef de query
* Toon het record en controleer of de gegevens correct zijn.
UPDATE customers
SET `address` = 'De van der veldensteeg 81',
`city` = 'Apeldoorn'
WHERE `id` = 13

#controle:
SELECT * FROM `customers` WHERE `id` = 13
#kolom is gewijzigd 