
#��1������������Ϣ

����#��2��ά������Ϣ

����#��3���������ָ��

#������4����������ʱִ��ָ�

#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

#������Ϣ
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base   #Linux Debian ���а��ϵ� .NET Core 3.1 ����ʱ
WORKDIR /app   # --ָ������Ŀ¼
EXPOSE 80      #ָ����Ҫ�����ṩ����Ķ˿�
EXPOSE 443   
#EXPOSE����ֻ������������Ӧ�ô򿪵Ķ˿ڲ�û��ʵ���Ͻ�����!
#Ҳ����˵������㲻��-p����-P��ָ��Ҫӳ��Ķ˿ڣ�
#��������ǲ���ӳ��˿ڳ�ȥ�ģ��Ӷ�����֪��������û�а취��Dockerfile������ж˿�ӳ��ģ�
#����ֻ��������������ʱ�������docker-compose�ļ���ʹ��ports��ָ����Ҫӳ��Ķ˿�

#����
FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["JenkinsDocker_Demo.csproj", ""]
RUN dotnet restore "./JenkinsDocker_Demo.csproj"     #dotnet restore - �ָ���Ŀ��������͹��ߡ�
COPY . .
WORKDIR "/src/."
RUN dotnet build "JenkinsDocker_Demo.csproj" -c Release -o /app/build

#����
FROM build AS publish
RUN dotnet publish "JenkinsDocker_Demo.csproj" -c Release -o /app/publish

#����
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "JenkinsDocker_Demo.dll"]