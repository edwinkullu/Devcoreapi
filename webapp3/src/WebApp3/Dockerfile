FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 5207

ENV ASPNETCORE_URLS=http://+:5207

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["webapp3/src/WebApp3/WebApp3.csproj", "webapp3/src/WebApp3/"]
RUN dotnet restore "webapp3/src/WebApp3/WebApp3.csproj"
COPY . .
WORKDIR "/src/webapp3/src/WebApp3"
RUN dotnet build "WebApp3.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "WebApp3.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApp3.dll"]
