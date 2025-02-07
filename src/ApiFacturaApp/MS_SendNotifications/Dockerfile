FROM mcr.microsoft.com/dotnet/runtime:8.0 AS base
USER $APP_UID
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["MS_SendNotifications/MS_SendNotifications.csproj", "MS_SendNotifications/"]
RUN dotnet restore "MS_SendNotifications/MS_SendNotifications.csproj"
COPY . .
WORKDIR "/src/MS_SendNotifications"
RUN dotnet build "MS_SendNotifications.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "MS_SendNotifications.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MS_SendNotifications.dll"]
