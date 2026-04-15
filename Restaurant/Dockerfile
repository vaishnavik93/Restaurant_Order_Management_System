# Stage 1: Build the backend application
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src

# Build context is the Restaurant/ folder (CI runs: cd Restaurant && docker build .)
# The actual .csproj lives in the Restaurant/ subfolder
COPY ["Restaurant/Restaurant.csproj", "Restaurant/"]
RUN dotnet restore "Restaurant/Restaurant.csproj"

COPY . .
WORKDIR "/src/Restaurant"
RUN dotnet build "Restaurant.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Restaurant.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Stage 2: Serve the application using the ASP.NET Core Runtime
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS runtime
WORKDIR /app
COPY --from=publish /app/publish .

EXPOSE 8080
ENV ASPNETCORE_HTTP_PORTS=8080

ENTRYPOINT ["dotnet", "Restaurant.dll"]
