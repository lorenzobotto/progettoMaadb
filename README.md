# Progetto MAADB
Progetto MAADB - Laurea Magistrale Informatica Unito

# Connettere MongoDB come Container Docker in VSCode
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

Scaricare l'ultima immagine di Postgres tramite il comando:
```
docker pull postgres
```

Creare un volume per avere i dati persistenti:
```
docker volume create --name=postgresdata
```

Stringa per runnare il container:
```
docker run --name postgresdb -v postgresdata:/data/db -d -p 5432:5432 -e POSTGRES_USER=admin -e POSTGRES_PASSWORD=admin -e POSTGRES_DB=maadb postgres
```

## VSCode
Scaricare ed installare l'estensione "MongoDB for VS Code" dal Marketplace di VSCode:

![Estensione MongoDB su VSCode](https://code.visualstudio.com/assets/docs/azure/mongodb/install-cosmosdb-extension.png)

Nella barra di sinistra, ci sarà una nuova icona di MongoDB. Selezionarla e aggiungere una nuova connessione:

![Aggiunta nuova connessione](https://code.visualstudio.com/assets/docs/azure/mongodb/cosmosdb-explorer.png)

Aggiungere una connessione tramite stringa di connessione:
```
mongodb://admin:admin@localhost:27017/admin
```

A questo punto la connessione è stabilita e si può iniziare a lavorare su MongoDB tramite VSCode, anche dal Playground.
