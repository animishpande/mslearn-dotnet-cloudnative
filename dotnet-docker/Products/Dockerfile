# Stage 1: Build the DataEntities project
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build

WORKDIR /DataEntities

# Copy and restore DataEntities project dependencies
COPY "DataEntities/DataEntities.csproj" .
RUN dotnet restore

# Copy the entire DataEntities project and publish it
COPY "DataEntities" .
RUN dotnet publish -c release -o /app

# Stage 2: Build the Products project
WORKDIR /dotnet-docker

# Copy and restore Products project dependencies
COPY Products/Products.csproj .
RUN dotnet restore

# Copy the entire Products project and publish it
COPY Products .
RUN dotnet publish -c release -o /app

# Stage 3: Create the runtime image
FROM mcr.microsoft.com/dotnet/aspnet:8.0

WORKDIR /app

# Expose ports for HTTP and HTTPS
EXPOSE 80
EXPOSE 443

# Copy the published output from the build stage
COPY --from=build /app .

# Set the entry point for the application
ENTRYPOINT ["dotnet", "Products.dll"]