## for docker container start and stop
- docker compose --env-file mongo_redis.env -f mongo_and_redis_dockercomposer_development.yml up -d
- docker compose --env-file mongo_redis.env -f mongo_and_redis_dockercomposer_development.yml down