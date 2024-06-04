FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 7172

ENV ASPNETCORE_URLS=http://+:7172
ENV ASPNETCORE_ENVIRONMENT=Development

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["ExchangeRates-MinimalAPI.csproj", "./"]
RUN dotnet restore "ExchangeRates-MinimalAPI.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "ExchangeRates-MinimalAPI.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "ExchangeRates-MinimalAPI.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ExchangeRates-MinimalAPI.dll"]