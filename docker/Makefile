SHELL := /bin/bash

default: run

run:
	source .custom.env; docker-compose -f docker-compose.yml up -d --force-recreate

log:
	source .custom.env; docker-compose -f docker-compose.yml logs -f

clean: 
	source .custom.env; docker-compose -f docker-compose.yml stop && docker-compose -f docker-compose.yml rm -f && docker volume prune -f && docker system prune -f

run-beers:
	source .custom.env; docker-compose -f docker-compose.beers.yml up -d

log-beers:
	source .custom.env; docker-compose -f docker-compose.yml -f docker-compose.beers.yml logs -f

clean-beers:
	source .custom.env; docker-compose -f docker-compose.beers.yml stop && docker-compose -f docker-compose.beers.yml rm -f && docker volume prune -f

clean-all: clean-beers clean

run-all: run run-beers

log-all: log log-beers