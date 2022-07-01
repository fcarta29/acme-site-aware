build:
	TAG=`git rev-parse --short=8 HEAD`; \
	docker build --rm -f Dockerfile -t harbor.tanzu.frankcarta.com/apps/acmeshop-catalog-aws-west:$$TAG .; \
	docker tag harbor.tanzu.frankcarta.com/apps/acmeshop-catalog-aws-west:$$TAG harbor.tanzu.frankcarta.com/apps/acmeshop-catalog-aws-west:latest

clean:
	docker stop acmeshop-catalog-aws-west
	docker rm acmeshop-catalog-aws-west

rebuild: clean build

run:
	# Run the MongoDB container
	docker run -d -p 27017:27017 --name acmeshop-catalog-db -e MONGO_INITDB_ROOT_USERNAME=mongoadmin -e MONGO_INITDB_ROOT_PASSWORD=secret -e MONGO_INITDB_DATABASE=acmefit gcr.io/vmwarecloudadvocacy/acmeshop-catalog-db

	# Run the user service
	docker run -d --name acmeshop-catalog-aws-west -e CATALOG_HOST=0.0.0.0 -e CATALOG_PORT=8082 -e CATALOG_VERSION=v1 -e CATALOG_DB_USERNAME=mongoadmin -e CATALOG_DB_PASSWORD=secret -e CATALOG_DB_HOST=0.0.0.0 CATALOG_DB_PORT=27017 -e USERS_HOST=0.0.0.0 -e USERS_PORT=8083 -p 8082:8082 harbor.tanzu.frankcarta.com/apps/acmeshop-catalog-aws-west:latest

start:
	docker start acmeshop-catalog-db
	docker start acmeshop-catalog-aws-west

stop:
	docker stop acmeshop-catalog-aws-west
	docker stop acmeshop-catalog-db

push:
	TAG=`git rev-parse --short=8 HEAD`; \
	docker push harbor.tanzu.frankcarta.com/apps/acmeshop-catalog-aws-west:$$TAG; \
	docker push harbor.tanzu.frankcarta.com/apps/acmeshop-catalog-aws-west:latest

default: build
