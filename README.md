# Manual run (Dev)

docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=example_123' -p 1433:1433 -d microsoft/mssql-server-linux
ASPNETCORE_ENVIRONMENT=Development dotnet run

# Local test

docker-compose build
docker-compose up

#

