FROM mcr.microsoft.com/dotnet/runtime:8.0 AS base
USER $APP_UID
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["MS_SendTaxesWorker/MS_SendTaxesWorker.csproj", "MS_SendTaxesWorker/"]
RUN dotnet restore "MS_SendTaxesWorker/MS_SendTaxesWorker.csproj"
COPY . .
WORKDIR "/src/MS_SendTaxesWorker"
RUN dotnet build "MS_SendTaxesWorker.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "MS_SendTaxesWorker.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MS_SendTaxesWorker.dll"]
