# db-videogames-query
-- 1 Selezionare tutte le software house americane (3)
SELECT * FROM software_houses WHERE country LIKE "United States";


-- 2 Selezionare tutti i giocatori della città di 'Rogahnland' (2)
SELECT * FROM players p WHERE city LIKE 'Rogahnland';

-- 3 Selezionare tutti i giocatori il cui nome finisce per "a" (220)
SELECT * FROM players p WHERE name LIKE  "%a";


-- 4- Selezionare tutte le recensioni scritte dal giocatore con ID = 800 (11)
SELECT * FROM reviews r WHERE player_id LIKE  "800";


-- 5- Contare quanti tornei ci sono stati nell'anno 2015 (9)
SELECT COUNT(*) from tournaments t WHERE `year` = "2019"; 


-- 6 Selezionare tutti i premi che contengono nella descrizione la parola 'facere' (2)
SELECT * FROM awards a WHERE description like "%facere%";


-- 7- Selezionare tutti i videogame che hanno la categoria 2 (FPS) o 6 (RPG), mostrandoli una sola volta (del videogioco vogliamo solo l'ID) (287)
SELECT DISTINCT videogame_id FROM category_videogame cv WHERE cv.category_id  = "2" OR cv.category_id = "6";

-- 8- Selezionare tutte le recensioni con voto compreso tra 2 e 4 (2947)
SELECT * FROM reviews r WHERE "2" <= r.rating AND r.rating  <= "4";

-- 9- Selezionare tutti i dati dei videogiochi rilasciati nell'anno 2020 (46)



-- 10- Selezionare gli id dei videogame che hanno ricevuto almeno una recensione da 5 stelle, mostrandoli una sola volta (443)
SELECT  DISTINCT videogame_id  FROM reviews r WHERE r.rating = 5;

-- *********** BONUS ***********

-- 11- Selezionare il numero e la media delle recensioni per il videogioco con ID = 412 (review number = 12, avg_rating = 3.16 circa)

-- 12- Selezionare il numero di videogame che la software house con ID = 1 ha rilasciato nel 2018 (13)





 -- GROUP BY

-- 1- Contare quante software house ci sono per ogni paese (3)
SELECT COUNT(*) FROM software_houses sh GROUP BY country; 

-- 2- Contare quante recensioni ha ricevuto ogni videogioco (del videogioco vogliamo solo l'ID) (500)
SELECT COUNT(*) FROM reviews r GROUP BY r.videogame_id; 

-- 3-  Contare quanti videogiochi hanno ciascuna classificazione PEGI (della classificazione PEGI vogliamo solo l'ID) (13)
SELECT COUNT(*) FROM pegi_label_videogame plv GROUP BY pegi_label_id; 

-- 4- Mostrare il numero di videogiochi rilasciati ogni anno (11)
SELECT year (v.release_date) , count(v.id) FROM videogames v 
GROUP BY year (v.release_date);

-- 5- Contare quanti videogiochi sono disponbiili per ciascun device (del device vogliamo solo l'ID) (7)
SELECT COUNT(*) FROM device_videogame dv GROUP BY device_id ; 

-- 6- Ordinare i videogame in base alla media delle recensioni (del videogioco vogliamo solo l'ID) (500)
SELECT AVG(rating) FROM reviews r group by videogame_id ORDER BY AVG(rating)ASC;




-- JOIN

-- 1- Selezionare i dati di tutti giocatori che hanno scritto almeno una recensione, mostrandoli una sola volta (996)

 SELECT DISTINCT p.id FROM players p 
 INNER JOIN reviews r 
 ON p.id = r.player_id;

-- oppure

SELECT DISTINCT player_id FROM reviews r 
inner join players p 
on r.player_id = p.id ;

-- 2- Sezionare tutti i videogame dei tornei tenuti nel 2016, mostrandoli una sola volta (226)
SELECT DISTINCT videogames.* -- seleziono uno per ogni videogames che verra mostrato 
	FROM videogames
	inner join tournament_videogame ON tournament_videogame.videogame_id = videogames.id -- seleziono tutti i videogiochi con relazione con la tabella tornei
	inner join tournaments ON tournaments.id = tournament_videogame.tournament_id -- seleziono tutti i tornei che hanno un id con relazione alla tabella tournament_videogame
	WHERE tournaments.`year` = 2016;

-- 3- Mostrare le categorie di ogni videogioco (1718)
SELECT * FROM categories c 
inner join category_videogame cv 
ON cv.category_id  = c.id;

-- 4- Selezionare i dati di tutte le software house che hanno rilasciato almeno un gioco dopo il 2020, mostrandoli una sola volta (6)
	SELECT DISTINCT software_houses.*
	FROM software_houses 
	inner join videogames
	ON software_houses.id = videogames.software_house_id
	WHERE year(videogames.release_date) > 2020;


-- 5- Selezionare i premi ricevuti da ogni software house per i videogiochi che ha prodotto (55)
SELECT awards.name AS awards_name,  videogames.name AS videogames_name, software_houses.name AS softwareHouse_name
	FROM awards
	inner join award_videogame ON award_videogame.award_id = awards.id
	inner join videogames ON videogames.id = award_videogame.videogame_id 
	inner join software_houses ON software_houses.id = videogames.software_house_id;

-- 6- Selezionare categorie e classificazioni PEGI dei videogiochi che hanno ricevuto recensioni da 4 e 5 stelle, mostrandole una sola volta (3363)
	SELECT DISTINCT v.name AS videogames_name, c.name AS categories_name, pl.name AS pegi_name
	FROM categories c
	inner join category_videogame cv ON cv.category_id = c.id
	inner join videogames v ON v.id = cv.videogame_id 
	inner join reviews r ON r.videogame_id = v.id
	inner join pegi_label_videogame plv ON plv.videogame_id = v.id 
	inner join pegi_labels pl ON pl.id = plv.pegi_label_id  
	where r.rating >= 4;


-- 7- Selezionare quali giochi erano presenti nei tornei nei quali hanno partecipato i giocatori il cui nome inizia per 'S' (474)

	SELECT DISTINCT v.*
	FROM videogames v 
	inner join tournament_videogame tv ON tv.videogame_id = v.id 
	inner join tournaments t ON t.id = tv.tournament_id 
	inner join player_tournament pt ON pt.tournament_id = t.id 
	inner join players p ON p.id = pt.player_id 
	WHERE p.name LIKE 'S%';


-- 8- Selezionare le città in cui è stato giocato il gioco dell'anno del 2018 (36)

-- 9- Selezionare i giocatori che hanno giocato al gioco più atteso del 2018 in un torneo del 2019 (3306)


-- *********** BONUS ***********

-- 10- Selezionare i dati della prima software house che ha rilasciato un gioco, assieme ai dati del gioco stesso (software house id : 5)

-- 11- Selezionare i dati del videogame (id, name, release_date, totale recensioni) con più recensioni (videogame id : potrebbe uscire 449 o 398, sono entrambi a 20)

-- 12- Selezionare la software house che ha vinto più premi tra il 2015 e il 2016 (software house id : potrebbe uscire 3 o 1, sono entrambi a 3)

-- 13- Selezionare le categorie dei videogame i quali hanno una media recensioni inferiore a 1.5 (10)
