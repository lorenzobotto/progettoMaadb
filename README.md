# Progetto MAADB
Progetto in Python (Jupyter Notebook) e MongoDB - Analisi Dataset Twitter - Corso di Modelli e Architetture Avanzati di Basi di Dati - Unito 2022.

## Descrizione Progetto

Viene analizzato il dataset dei tweet tramite Jupyter Notebook (Python) e l'utilizzo di un database NoSQL (MongoDB) e un database relazionale (PostgreSQL). Ad ogni messaggio è associato un sentimento che si suppone sia il sentimento espresso dal messaggio.

Tale dataset consiste in un grande numero di messaggi reali di utenti, scambiati sulla popolare piattaforma di microblogging Twitter (detti tweets). 
Si richiede di trovare quali parole sono le più frequenti per ogni sentimento. I messaggi sono in inglese.
Per ottenere questo risultato vengono svilupatte due soluzioni software che rispettivamente utilizzeranno due diversi database, per mettere a confronto le rispettive soluzioni applicative: 
- con l’utilizzo del database relazionale PostgreSQL;
- con l'utilizzo del database NOSQL: MongoDB. 
In questo si modo si valuta quale delle due soluzioni è migliore e per MongoDB si utilizza il paragdima di progettazione delle Pipeline.

Come input si avranno:
- risorse lessicali per ogni sentimento, con una lista delle parole per quel sentimento;
- dataset di tweets: il dataset consiste in un insieme di otto files contenenti ciascuno sessantamila messaggi reali di utenti, scambiati su Twitter (detti tweets). I file sono denominati con un sentimento associato ai messaggi. 

### Come sono stati trattati i tweet

Queste sono le operazioni effettuate sui tweet per elaborarli:
- pulizia dei tweet dalle parole URL ed USERNAME, con successivo riconoscimento degli hashtag e conteggio (conteggio tramite Pipeline di Mongo e DB Relazione);
- riconoscimento delle emoji e delle emoticons contandone la presenza nei vari sentimenti/tweet (conteggio tramite Pipeline di Mongo e DB Relazione);
- riconoscimento degli slangs e modifica delle slang con la definizione;
- tokenizzazione di tutte le frasi;
- esecuzione del POS Tagging;
- trattamento della punteggiatura;
- esecuzione della lemmatization per permettere il match con le risorse lessicali;
- trattamento stop words;
- conteggio della presenza nei vari tweet delle parole associate a un determinato sentimento;
- memorizzazione delle 'nuove' parole trovate nei tweet ma assenti nelle risorse di input.

### Risultati prodotti

Questi sono i risultati finali:
- per ogni sentimento è stata creata una word cloud con le parole maggiormente presenti nei tweet (la grandezza del carattere nella word cloud è proporzionale alla frequenza nei messaggi tweet).
- è stata creata una word cloud anche per le emoji, le emoticons e gli hashtags.
- vengono calcolate, tramite Pipeline di Mongo e DB Relazione, due percentuali per ogni risorsa lessicale come mostrato nella figura sottostante:

![Immagine Percentuali](http://drive.google.com/uc?export=view&id=1-Ach8t9DVPMIQzrbxbn9655aKo2T5f6H)

Nella figura si mostrano due insiemi di parole che sono il risultato dell’elaborazione delle due sorgenti (risorse lessicali X per un determinato sentimento Y, in cui il numero totale delle parole è N_lex_words(X) e messaggi tweets etichettati con lo stesso sentimento Y, in cui il numero totale delle parole è N_twitter_words(Y). L’intersezione tra i due insiemi contiene le parole comuni N_shared_words(X,Y) e le due formule mostrano come sono state calcolate le percentuali. 
- è stato creato un istogramma per ciascun sentimento con le due percentuali calcolate precedentemente. 
- sono state raccolte le parole “nuove” presenti nella sorgente Tweet ma non nelle risorse lessicali (cartella "Nuove Parole").

### Come sono state trattate le risorse lessicali

Questi sono le operazioni effettuate:
- sono state eliminate dalle risorse le parole composte (con "_");
- sono state inserite all'interno del database.

# Come eseguire

## Installare librerie

Prima di iniziare è necessario installare tutte le librerie richieste (import della prima cella) per eseguire la prima cella che carica tutto il necessario, installandole tramite:
```
pip install <nomeLibreria>
```

## Connettere MongoDB come Container Docker
## Docker
### Mongo
Scaricare l'ultima immagine di MongoDB tramite il comando:
```
docker pull mongo
```
Creare un volume per avere i dati persistenti:
```
docker volume create --name=mongodata
```
Eseguire il container:
```
docker run --name mongodb -v mongodata:/data/db -d -p 27017:27017 mongo
```
Creare l'utente per l'autenticazione, spostandosi nella bash, successivamente nel mongo management ed infine nel database admin:
```
docker exec -it mongodb bash
mongo
use admin
```
Creare l'utente con la password:
```
db.createUser({
  user: "admin", 
  pwd: "admin", 
  roles: [ { role: "root", db: "admin" } ]
})
```

A questo punto si possono creare database e collezioni.

### Postgres

Scaricare Postgres in locale da: 

## VSCode per visualizzare le modifiche
Scaricare ed installare l'estensione "MongoDB for VS Code" dal Marketplace di VSCode per visualizzare tutte gli inserimenti/modifiche:

![Estensione MongoDB su VSCode](https://code.visualstudio.com/assets/docs/azure/mongodb/install-cosmosdb-extension.png)

Nella barra di sinistra, ci sarà una nuova icona di MongoDB. Selezionarla e aggiungere una nuova connessione:

![Aggiunta nuova connessione](https://code.visualstudio.com/assets/docs/azure/mongodb/cosmosdb-explorer.png)

Aggiungere una connessione tramite stringa di connessione:
```
mongodb://admin:admin@localhost:27017/admin
```

A questo punto la connessione è stabilita e si può iniziare a lavorare su MongoDB tramite VSCode, anche dal Playground.
