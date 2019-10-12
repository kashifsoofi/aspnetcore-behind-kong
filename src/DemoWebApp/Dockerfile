FROM mcr.microsoft.com/dotnet/core/sdk:2.2-alpine AS build
WORKDIR /build

# copy csproj and restore as distinct layers
COPY *.sln ./

# Copy the main source project files
COPY src/*/*.csproj ./
RUN for file in $(ls *.csproj); do mkdir -p src/${file%.*}/ && mv $file src/${file%.*}/; done

# Restore to cache the layers
RUN dotnet restore ./src/DemoWebApp/DemoWebApp.csproj

# Copy all the source code and build
COPY ./src ./src

RUN dotnet build ./src/DemoWebApp/DemoWebApp.csproj -c Release --no-restore

FROM build AS publish
RUN dotnet publish ./src/DemoWebApp/DemoWebApp.csproj -c Release -o /app --no-restore --no-build

# App image
FROM mcr.microsoft.com/dotnet/core/aspnet:2.2-alpine AS runtime
WORKDIR /app
COPY --from=publish /app ./
ENTRYPOINT ["dotnet", "DemoWebApp.dll"]