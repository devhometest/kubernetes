FROM mcr.microsoft.com/dotnet/core/aspnet:2.2 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build
WORKDIR /src
COPY ["kubernetes/kubernetes.csproj", "kubernetes/"]

RUN dotnet restore "kubernetes/kubernetes.csproj"
COPY . .
WORKDIR "/src/kubernetes"
RUN dotnet build "kubernetes.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "kubernetes.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "kubernetes.dll"]