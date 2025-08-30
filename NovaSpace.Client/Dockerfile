# Usar la imagen de .NET 10 Preview SDK para build
FROM mcr.microsoft.com/dotnet/sdk:10.0-preview AS build
WORKDIR /src
COPY ["NovaSpace.Client/NovaSpace.Client.csproj", "NovaSpace.Client/"]
RUN dotnet restore "NovaSpace.Client/NovaSpace.Client.csproj"
COPY . .
WORKDIR "/src/NovaSpace.Client"
RUN dotnet build "NovaSpace.Client.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "NovaSpace.Client.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Usar nginx para servir la aplicación Blazor WebAssembly
FROM nginx:alpine AS final
WORKDIR /usr/share/nginx/html
COPY --from=publish /app/publish/wwwroot .
COPY NovaSpace.Client/nginx.conf /etc/nginx/nginx.conf
EXPOSE 80
EXPOSE 443