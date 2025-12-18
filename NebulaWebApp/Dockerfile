FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Копируем только csproj, чтобы restore кэшировался
COPY ["NebulaWebApp/NebulaWebApp.csproj", "NebulaWebApp/"]
RUN dotnet restore "NebulaWebApp/NebulaWebApp.csproj"

# Копируем остальной исходник
COPY . .
WORKDIR "/src/NebulaWebApp"
RUN dotnet publish "NebulaWebApp.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
ENV ASPNETCORE_URLS=http://+:8080
EXPOSE 8080

COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "NebulaWebApp.dll"]
