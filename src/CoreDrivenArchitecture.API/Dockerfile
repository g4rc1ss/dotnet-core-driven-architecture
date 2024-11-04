FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["src/CoreDrivenArchitecture.API/CoreDrivenArchitecture.API.csproj", "src/CoreDrivenArchitecture.API/"]
COPY ["src/CoreDrivenArchitecture.UseCases/CoreDrivenArchitecture.UseCases.csproj", "src/CoreDrivenArchitecture.UseCases/"]
COPY ["src/CoreDrivenArchitecture.Data/CoreDrivenArchitecture.Data.csproj", "src/CoreDrivenArchitecture.Data/"]
COPY ["src/CoreDrivenArchitecture.DTOs/CoreDrivenArchitecture.DTOs.csproj", "src/CoreDrivenArchitecture.DTOs/"]
COPY ["src/CoreDrivenArchitecture.Notificator/CoreDrivenArchitecture.Notificator.csproj", "src/CoreDrivenArchitecture.Notificator/"]
RUN dotnet restore "src/CoreDrivenArchitecture.API/CoreDrivenArchitecture.API.csproj"
COPY . .
WORKDIR "/src/src/CoreDrivenArchitecture.API"
RUN dotnet build "CoreDrivenArchitecture.API.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "CoreDrivenArchitecture.API.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CoreDrivenArchitecture.API.dll"]
