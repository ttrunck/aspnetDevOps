# How to develop the service

To develop the service you will need a MSSQL server. You can use docker to create one with the following 

```
docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=example_123' -p 1433:1433 -d microsoft/mssql-server-linux
```

To run your code with the development config you can use the following
```
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

The service should be accessible on `http://localhost:5000` and the healcheck (this will also test the DB is accessible) is on `http://localhost:5000/health`

# Unit test

We don't have unit test (yet) but when we will have them they will be integrate to docker build. So you should just build the docker service image with `docker build .`. Note that before merging a pull request the CI will check that the docker build is passing. That how unit will be covered

# Integration test

Because those are expensive (in a production app), integration tests aren't required to merge a pull request. But those are ran continuouly by the CI on the `main` branch. To reduce cost, all the current change are batched.
Because this is just a sample app, here we only check the app health endpoint. This tests that the service is up and can reach the DB.

To run the tests locally you can do the following

```
docker-compose build
docker-compose up
wget --spider --no-verbose --retry-connrefused --tries=5 http://localhost/health  || exit 1
```
# Deployment

We have two environments `ring0` is a testing ring which is updated automatically each time the main branch pass the integration tests. `ring1` is the production ring, and you need to manually approve that the deployment on ring0 can be deployed to ring1.
