## Minimal 3 tier web application

- **React frontend:** Uses react query to load data from the two apis and display the result.
- **Node.js and Go APIs:** Both have `/` and `/ping` endpoints. `/` queries the Database for the current time and the number of requests for each api recorded within the database, and `/ping` returns `pong`.
- **Postgres Database:** An empty PostgreSQL database with no tables or data. Used to show how to set up connectivity. The API applications execute `SELECT NOW() as now;` to determine the current time to return.
- **Python Load Generator:** Queries one of either the Node.js or Go APIs at a configurable speed. It adds a simple load generator written in python and adds a simple database schema to track requests to each API.

## Running the Application

While the whole point of this course is that you probably won't want/need to run the application locally, we can do so as a starting point.

### Postgres

It's way more convenient to run postgres in a container, so we will do that.

`docker run -e POSTGRES_PASSWORD=password -v pg-api-data:/var/lib/postgresql/data -p 6543:5432 postgres:16.3-alpine` will start postgres in a container and publish port 5432 -> 6543 from the container to your localhost.

**🚨 NOTE:** After starting the database, you need to run the migration file in `./migrations` to create the table that the APIs use. This can be done with `postgresql:run-psql-init-script`.

### api-node

To run the node api you will need to `npm install` to install the dependencies.

After installing the dependencies, `DATABASE_URL={{.DATABASE_URL}} npm run dev` will run the api in development mode with nodemon for restarting the app when you make source code changes.

### api-golang

To run the golang api you will need to run `go mod tidy`.

After installing the dependencies, `DATABASE_URL={{.DATABASE_URL}} go run main.go` will build and run the api.

### client-react

Like api-node, you will first need to install the dependencies with `npm install`.

After installing the dependencies, `npm run dev` will use vite to run the react app in development mode.

### load-generator-python

This service uses poetry to manage dependencies. You will need to install the dependencies with `poetry install --no-root`.

After installing the dependencies, `API_URL=http://localhost:8000/ DELAY_MS=100 poetry run python main.py` will run the application.
