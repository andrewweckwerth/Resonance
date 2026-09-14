# Legacy Development Environment Setup Instructions

VS Code might throw q fit if you don’t have the required Python packages installed yet.  
Some dependencies only work correctly on **Python 3.12**, so make sure you’re using that version.

Check your Python version:

```bash
python -V
```

Create python virtual environment:

```bash
python3.12 -m venv .venv
```

Activate python virtual environment:

```bash
source .venv/bin/activate
```

install packages:

```bash
pip install -r backend/requirements.txt
```

Create a .env file and then copy .env.example to .env. Docker will use .env to store all of our passwords and keys. However, we can't expose these keys so .env will have to be gitignore. For now, since all the services are hosted locally and have dummy data, we can use expose passwords. Copy the entire .env.example file into .env and make sure to save the file.


Make sure Docker Desktop is installed and running:
https://www.docker.com/products/docker-desktop/

Run ```docker compose up```. This is will start each of the containers
* frontend: http://localhost:3000/
* backend: http://localhost:8000/
* minio web console: http://localhost:9001/ 
* minio object store: http://localhost:9000/
* postgres web console: http://localhost:5050/
* postgres database: http://localhost:5432/

For now the data will be stored locally, but later it will be hosted. This means you have to create the schemas locally.

For Postgres:

1. Create the tables from the schema:

```bash
docker compose exec -T postgres psql -U app -d appdb < db/schema.sql
```

2. Open pgAdmin at:

http://localhost:5050/

Login using the email and password from `.env`.

3. Connect to the database:
 
Register a new server with:

- Host: `postgres`
- Port: `5432`
- Database: `appdb`
- Username: `app`
- Password: `apppassword`

4. View the tables:

The screenshot below shows an example of viewing the `presets` table.
![alt text](<docs/Screenshot 2026-01-21 at 8.47.19 PM.png>)

For Minio:
Go to http://localhost:9001/ and login with the user and password from the docker-compose.yml file. Click create bucket and name it synthgpt. the presets and renderings are stored as:
* synthgpt/presets/<preset_id>/preset.vital
* synthgpt/presets/<preset_id>/preview.wav

The backend accesses the objects with:
* Endpoint: minio:9000
* Access Key: minioadmin
* Secret Key: minioadmin123
* Bucket: synthgpt
