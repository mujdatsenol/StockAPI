#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Presentation/Stock.API/Stock.API.csproj", "Presentation/Stock.API/"]
COPY ["Application/Stock.Application/Stock.Application.csproj", "Application/Stock.Application/"]
COPY ["Domain/Stock.Domain/Stock.Domain.csproj", "Domain/Stock.Domain/"]
COPY ["Application/Stock.Application.Abstractions/Stock.Application.Abstractions.csproj", "Application/Stock.Application.Abstractions/"]
COPY ["Infrastructure/Stock.Common/Stock.Common.csproj", "Infrastructure/Stock.Common/"]
COPY ["Infrastructure/Stock.Infrastructure/Stock.Infrastructure.csproj", "Infrastructure/Stock.Infrastructure/"]
COPY ["Infrastructure/Stock.Persistence/Stock.Persistence.csproj", "Infrastructure/Stock.Persistence/"]
RUN dotnet restore "Presentation/Stock.API/Stock.API.csproj"
COPY . .
WORKDIR "/src/Presentation/Stock.API"
RUN dotnet build "Stock.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Stock.API.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Stock.API.dll"]