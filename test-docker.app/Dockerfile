#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM  microsoft/dotnet:2.1-aspnetcore-runtime AS base
WORKDIR /app
EXPOSE 58366
EXPOSE 44396

FROM microsoft/dotnet:2.1-sdk AS build
WORKDIR /src
COPY ["test-docker.app/test-docker.app.csproj", "test-docker.app/"]
RUN dotnet restore "test-docker.app/test-docker.app.csproj" 
COPY . .
WORKDIR "/src/test-docker.app"
RUN dotnet build "test-docker.app.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "test-docker.app.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "test-docker.app.dll"]