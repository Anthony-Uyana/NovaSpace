# Usar la imagen base de .NET 10 Preview para runtime
FROM mcr.microsoft.com/dotnet/aspnet:10.0-preview AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

# Usar la imagen de .NET 10 Preview SDK para build
FROM mcr.microsoft.com/dotnet/sdk:10.0-preview AS build
WORKDIR /src
COPY ["NovaSpace.API/NovaSpace.API.csproj", "NovaSpace.API/"]
RUN dotnet restore "NovaSpace.API/NovaSpace.API.csproj"
COPY . .
WORKDIR "/src/NovaSpace.API"
RUN dotnet build "NovaSpace.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "NovaSpace.API.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "NovaSpace.API.dll"]