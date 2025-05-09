# Django Celery Redis Flower Beat Docker Project

This project is a Django application set up for scalable development and production using Docker. It includes:

- Django app server
- PostgreSQL database
- Redis for caching and Celery broker
- Celery worker for background tasks
- Celery Beat for scheduled tasks
- Flower for monitoring Celery tasks

## Project Structure

```
.
├── a_core/           # Django core settings and configuration
├── a_home/           # Django app: home
├── a_users/          # Django app: users
├── static/           # Static files
├── staticfiles/      # Collected static files
├── templates/        # Django templates
├── postgres_data/    # PostgreSQL data (Docker volume)
├── redis_data/       # Redis data (Docker volume)
├── manage.py
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
└── README.md
```

## Prerequisites

- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/)

## Environment Variables

All services use the environment variables defined in [`a_core/.env`](a_core/.env).

Example `.env`:

```
DEBUG=1
SECRET_KEY=your-secret-key
DJANGO_ALLOWED_HOSTS=localhost 127.0.0.1 [::1]
DATABASE_URL=postgres://postgres:postgres@postgres:5432/postgres
REDIS_URL=redis://my_redis:6379/0
```

## Getting Started

### 1. Build and Start All Services

```sh
docker-compose up --build
```

This will start:

- Django app server on [http://localhost:8001](http://localhost:8001)
- PostgreSQL on port 5432
- Redis on port 6379
- Celery worker
- Celery Beat
- Flower on [http://localhost:5555](http://localhost:5555) (login: `admin:password123`)

### 2. Running Migrations

Migrations are run automatically when the app container starts.

To run manually:

```sh
docker-compose exec app python manage.py makemigrations
docker-compose exec app python manage.py migrate
```

### 3. Creating a Superuser

```sh
docker-compose exec app python manage.py createsuperuser
```

### 4. Collect Static Files

```sh
docker-compose exec app python manage.py collectstatic --noinput
```

### 5. Accessing the Application

- Django app: [http://localhost:8001](http://localhost:8001)
- Flower dashboard: [http://localhost:5555](http://localhost:5555)

## Useful Commands

- **Stop all services:**
  ```sh
  docker-compose down
  ```
- **Rebuild containers:**
  ```sh
  docker-compose up --build
  ```
- **View logs:**
  ```sh
  docker-compose logs -f
  ```

## Services Overview

| Service   | Description                       | Port(s)   |
|-----------|-----------------------------------|-----------|
| app       | Django development server         | 8001      |
| postgres  | PostgreSQL database               | 5432      |
| redis     | Redis server                      | 6379      |
| celery    | Celery worker                     | -         |
| beat      | Celery Beat scheduler             | -         |
| flower    | Celery monitoring dashboard       | 5555      |

## Notes

- Data for PostgreSQL and Redis is persisted in `postgres_data/` and `redis_data/` directories.
- All services use the same Docker image built from your `Dockerfile`.
- Flower is protected with basic auth (`admin:password123`).

## License

See [LICENSE](staticfiles/admin/img/LICENSE) for icon assets. Project code is under your preferred license.

---

**Happy coding!**