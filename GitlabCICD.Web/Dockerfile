FROM mcr.microsoft.com/dotnet/core/sdk:3.0-alpine AS build
WORKDIR /src

COPY GitlabCICD.Web/GitlabCICD.Web.csproj GitlabCICD.Web/
COPY GitlabCICD.Test/GitlabCICD.Test.csproj GitlabCICD.Test/
RUN dotnet restore GitlabCICD.Web/GitlabCICD.Web.csproj
COPY . .

WORKDIR /src/GitlabCICD.Test
RUN dotnet test GitlabCICD.Test.csproj -c Release -o /app

WORKDIR /src/GitlabCICD.Web
FROM build AS publish
RUN dotnet publish GitlabCICD.Web.csproj -c Release -o /app -r linux-x64 --self-contained=false

FROM mcr.microsoft.com/dotnet/core/aspnet:3.0-alpine AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "GitlabCICD.Web.dll"]
