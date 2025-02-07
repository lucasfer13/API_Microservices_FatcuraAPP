FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["MS_ApiProducts/MS_ApiProducts.csproj", "MS_ApiProducts/"]
RUN dotnet restore "MS_ApiProducts/MS_ApiProducts.csproj"
COPY . .
WORKDIR "/src/MS_ApiProducts"
RUN dotnet build "MS_ApiProducts.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "MS_ApiProducts.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MS_ApiProducts.dll"]
