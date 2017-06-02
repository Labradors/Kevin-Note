<Context path="" docBase="tigase" debug="0" reloadable="true"/>



<Connector

     port="9090"
     protocol="org.apache.coyote.http11.Http11Nio2Protocol"
     connectionTimeout="20000"
     redirectPort="8443"
     enableLookups="false"
     acceptCount="500"
     maxThreads="1000"
     minSpareThreads="10"
     compression="on"
     compressionMinSize="2048"
     compressableMimeType="text/html,text/xml,text/plain,text/css,text/javascript,application/javascript"
     URIEncoding="utf-8"
  />



<Executor

        name="tomcatThreadPool"
        namePrefix="catalina-exec-"
        maxThreads="500"
        minSpareThreads="30"
        maxIdleTime="60000"
        prestartminSpareThreads = "true"
        maxQueueSize = "100"
    />
