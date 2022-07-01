build:
	TAG=`git rev-parse --short=8 HEAD`; \
	docker build --rm -f Dockerfile -t harbor.tanzu.frankcarta.com/apps/acmeshop-catalog-aws-east:$$TAG .; \
	docker tag harbor.tanzu.frankcarta.com/apps/acmeshop-catalog-aws-east:$$TAG harbor.tanzu.frankcarta.com/apps/acmeshop-catalog-aws-east:latest

clean:
	docker stop acmeshop-catalog-aws-east
	docker rm acmeshop-catalog-aws-east

rebuild: clean build

run:
	# Run the MongoDB container
	docker run -td -p 27017:27017 --name acmeshop-catalog-aws-east-db -e MONGO_INITDB_ROOT_USERNAME=mongoadmin -e MONGO_INITDB_ROOT_PASSWORD=secret -e MONGO_INITDB_DATABASE=acmefit gcr.io/vmwarecloudadvocacy/acmeshop-catalog-db

	# Run the user service
	docker run -td --name acmeshop-catalog-aws-east -e CATALOG_HOST=0.0.0.0 -e CATALOG_PORT=8082 -e CATALOG_VERSION=v1 -e CATALOG_DB_USERNAME=mongoadmin -e CATALOG_DB_PASSWORD=secret -e CATALOG_DB_HOST=0.0.0.0 -e USERS_HOST=0.0.0.0 -e USERS_PORT=8083 -p 8082:8082 harbor.tanzu.frankcarta.com/apps/acmeshop-catalog-aws-east:latest

start:
	docker start acmeshop-catalog-db
	docker start acmeshop-catalog-aws-east

stop:
	docker stop acmeshop-catalog-aws-east
	docker stop acmeshop-catalog-db

push:
	TAG=`git rev-parse --short=8 HEAD`; \
	docker push harbor.tanzu.frankcarta.com/apps/acmeshop-catalog-aws-east:$$TAG; \
	docker push harbor.tanzu.frankcarta.com/apps/acmeshop-catalog-aws-east:latest

default: build
