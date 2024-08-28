FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 5177

ENV ASPNETCORE_URLS=http://+:5177

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["webapp2/src/WebApp2/WebApp2.csproj", "webapp2/src/WebApp2/"]
RUN dotnet restore "webapp2/src/WebApp2/WebApp2.csproj"
COPY . .
WORKDIR "/src/webapp2/src/WebApp2"
RUN dotnet build "WebApp2.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "WebApp2.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApp2.dll"]
