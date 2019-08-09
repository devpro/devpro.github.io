# Cloud Foundry CLI cheatsheet

[Home](../readme.md) > Cloud Foundry > [Cloud Foundry CLI cheatsheet](./cloudfoundry-cli-cheatsheet.md)

`cf version`

`cf --version`

`cf --help`

`cf <command> -h`

`cf login -a <cf-api-endpoint>`

`cf target`

`cf push pal-tracker --random-route -p src/PalTracker/bin/Release/netcoreapp2.1/publish`

`cf set-env pal-tracker WELCOME_MESSAGE "Hello from Cloud Foundry"`

`cf restart pal-tracker`

`cf delete pal-tracker`

`cf map-route`

`cf marketplace`

`cf create-service cleardb spark tracker-database`

`cf bind-service pal-tracker tracker-database`
