# apache tomcat

```
docker run -it --rm -p 8787:8080 \
-v /Users/kihyuckhong/Documents/Docker/Tomcat/sample.war:/usr/local/tomcat/webapps/sample.war \
tomcat
```

```
curl http://localhost:8787
```

*Dockerfile*

```
FROM tomcat:8.5.35-jre10
ADD sample.war /usr/local/tomcat/webapps/
EXPOSE 8080
CMD chmod +x /usr/local/tomcat/bin/catalina.sh
CMD ["catalina.sh", "run"]
```

```
docker build -t mytomcat .
docker run -it -p 8787:8080 mytomcat
```
