FROM mcr.microsoft.com/dotnet/core/sdk:2.2-alpine AS build

WORKDIR /build
COPY src/DemoWebApp/DemoWebApp.csproj src/DemoWebApp/
RUN dotnet restore "src/DemoWebApp/DemoWebApp.csproj"
COPY . .
WORKDIR "/build/src/DemoWebApp"
RUN dotnet build "DemoWebApp.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "DemoWebApp.csproj" -c Release -o /app/publish

FROM mcr.microsoft.com/dotnet/core/aspnet:2.2-alpine AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DemoWebApp.dll"]
