#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["Directory.Build.props", "."]
COPY ["TagzApp.Web/TagzApp.Web.csproj", "TagzApp.Web/"]
COPY ["TagzApp.Common/TagzApp.Common.csproj", "TagzApp.Common/"]
COPY ["TagzApp.Providers.Mastodon/TagzApp.Providers.Mastodon.csproj", "TagzApp.Providers.Mastodon/"]
COPY ["TagzApp.Providers.Twitter/TagzApp.Providers.Twitter.csproj", "TagzApp.Providers.Twitter/"]
RUN dotnet restore "TagzApp.Web/TagzApp.Web.csproj"
COPY . .
WORKDIR "/src/TagzApp.Web"
RUN dotnet build "TagzApp.Web.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "TagzApp.Web.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TagzApp.Web.dll"]