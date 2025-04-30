### Other misc configs

Logger implementation:
```properties
# Logger implementation - SystemLogger logs into System.out/err
# replace with logging implementation of your choice
# Slf4JLogger can be configured in {TOMCAT-HOME}/lib/log4j2.xml
# oskari.logger=fi.nls.oskari.log.SystemLogger
oskari.logger=fi.nls.oskari.utils.Log4JLogger
```

Where to find terms of use, user guide etc documents
```properties
# "CMS content" files location
actionhandler.GetArticlesByTag.dir=/articlesByTag/
```

For debugging service connections and TLS problems (self-signed sertificates etc)

```properties
# true all ssl certs/hosts for debugging! configure certs on the server for production
oskari.trustAllCerts=true
# true all ssl certs/hosts for debugging! configure certs on the server for production
oskari.trustAllHosts=true
```
