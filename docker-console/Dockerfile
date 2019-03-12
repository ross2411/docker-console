#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM microsoft/dotnet:2.2-runtime-nanoserver-1809 AS base
WORKDIR /app


FROM microsoft/dotnet:2.2-sdk-nanoserver-1809 AS build
WORKDIR /src
COPY ["docker-console/docker-console.csproj", "docker-console/"]
RUN dotnet restore "docker-console/docker-console.csproj"
COPY . .
WORKDIR "/src/docker-console"
RUN dotnet build "docker-console.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "docker-console.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "docker-console.dll"]