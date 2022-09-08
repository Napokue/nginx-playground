#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["scenarios/scenario-http-proxy/src/scenario-http-proxy-web-api/scenario-http-proxy-web-api.csproj", "scenarios/scenario-http-proxy/src/scenario-http-proxy-web-api/"]
RUN dotnet restore "scenarios/scenario-http-proxy/src/scenario-http-proxy-web-api/scenario-http-proxy-web-api.csproj"
COPY . .
WORKDIR "/src/scenarios/scenario-http-proxy/src/scenario-http-proxy-web-api"
RUN dotnet build "scenario-http-proxy-web-api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "scenario-http-proxy-web-api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "scenario-http-proxy-web-api.dll"]