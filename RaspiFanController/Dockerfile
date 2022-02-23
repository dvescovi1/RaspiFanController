#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
# [Optional] Uncomment this section to install additional OS packages.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends libgpiod-dev

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["RaspiFanController/RaspiFanController.csproj", "RaspiFanController/"]
RUN dotnet restore "RaspiFanController/RaspiFanController.csproj"
COPY . .
WORKDIR "/src/RaspiFanController"
RUN dotnet build "RaspiFanController.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "RaspiFanController.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "RaspiFanController.dll"]