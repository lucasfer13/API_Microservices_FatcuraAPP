FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["MS_ApiBilling/MS_ApiBilling.csproj", "MS_ApiBilling/"]
RUN dotnet restore "MS_ApiBilling/MS_ApiBilling.csproj"
COPY . .
WORKDIR "/src/MS_ApiBilling"
RUN dotnet build "MS_ApiBilling.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "MS_ApiBilling.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MS_ApiBilling.dll"]
