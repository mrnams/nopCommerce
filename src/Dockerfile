FROM microsoft/dotnet:2.2-sdk AS build                                          
WORKDIR /src                                                                    

# copy csproj and restore as distinct layers
COPY . ./

RUN dotnet restore NopCommerce.sln

WORKDIR /src/Presentation/Nop.Web      
RUN dotnet build Nop.Web.csproj -c Release -o /app                                         
#RUN dotnet publish -c Release -o /app   

FROM build AS publish
RUN dotnet publish Nop.Web.csproj -c Release -o /app

WORKDIR /app  
RUN mkdir bin
RUN mkdir logs  

FROM microsoft/dotnet:2.2-aspnetcore-runtime-alpine AS runtime                                    
WORKDIR /app                                                                    
COPY --from=publish /app .
                            
ENTRYPOINT ["dotnet", "Nop.Web.dll"]

