
om pakken ikke er alpine kan man bruke:
`docker run --rm mcr.microsoft.com/dotnet/aspnet:latest dpkg -l | grep "perl"`

for og se etter perl for eksempel


eller dotnet runtimes
`docker run --rm mcr.microsoft.com/dotnet/aspnet:latest dotnet --list-runtimes`
