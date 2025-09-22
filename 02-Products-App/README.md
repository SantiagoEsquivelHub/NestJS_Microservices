## Dev 

1. Clone the repository
2. Create a .env based on the .env.template
3. Run the `git submodule update --init --recursive` command to rebuild the submodules
4. Run the `docker compose up --build` command

## Prod

1. Clone the repository
2. Create a .env based on the .env.template
3. Run the `docker compose -f docker-compose.prod.yaml build` command