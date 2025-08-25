# Emulate environment variables from .env file
DATABASE_URL:=postgres://postgres:foobarbaz@localhost:5432/postgres

# Run postgres container
.PHONY: run-postgres
run-postgres:
	@echo Starting postgres container
	-docker run \
		-e POSTGRES_PASSWORD=foobarbaz \
		-v pgdata:/var/lib/postgresql/data \
		-p 5432:5432 \
		postgres:15.1-alpine

# Run node api
.PHONY: run-api-node
run-api-node:
	@echo Starting node api
	cd api-node && \
		DATABASE_URL=${DATABASE_URL} \
		npm run dev

# Run golang api
.PHONY: run-api-golang
run-api-golang:
	@echo Starting golang api
	cd api-golang && \
		DATABASE_URL=${DATABASE_URL} \
		go run main.go

# Run client react
.PHONY: run-client-react
run-client-react:
	@echo Starting react client
	cd client-react && \
		npm run dev
