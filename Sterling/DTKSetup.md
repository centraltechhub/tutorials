**DTK Installation**

1. Download the latest DTK toolkit (devtoolkit_docker_10.0.2209.0.tar) from IBM selfserve tool https://selfservice.oms.supply-chain.ibm.com/selfserve.

2. Extract the tar file. The exploded folder will contain the following sub-folders and files.

![image](https://user-images.githubusercontent.com/93929892/211470566-dbf2d5df-4d12-4142-9302-c0b978e5e380.png)

3. Navigate to compose directory. Make a copy of om-compose.properties.sample to om-compose.properties. Add the following configuration:

```PROP
DTK_LICENSE=accept
OM_INSTALL_LOCALIZATION=true
OM_LOCALES=zh_CN,zh_TW,fr_FR,ja_JP,pt_BR,ko_KR,ru_RU,tr_TR,it_IT,es_ES,de_DE,pl_PL
LOCALE_CODE=en_US_UTC
AP_WAR_FILES=wscdev,smcfs,sbc,sma,isccsdev,isf
SBA_ENABLE=Y
SIM_IV_ENABLE=N
IV_ENABLE=N
HOST_OS=mac
JAVA_HOME=/Users/hussamoa/Techhub/DTK/jdk
```
4. Copy your base Operating system compatible JDK to the location configured in the JAVA_HOME property in the previous step.

In this example, I am using the following:

![image](https://user-images.githubusercontent.com/93929892/211471281-05693b79-7f41-4a35-8878-c7cc5e11f211.png)

5. Navigate to `/devtoolkit_docker/compose/docker/appserver and update the jvm.options as follows:

```PROP
-Dvendor=shell
-DvendorFile=servers.properties
-Xms4096m
-Xmx4096m
-Dhttps.protocols=TLSv1.2
-Dcom.ibm.jsse2.overrideDefaultTLS=true
-Djavax.net.ssl.keyStore=/var/oms/keystore/key.jks
-Djavax.net.ssl.keyStorePassword=secret4ever
-Djavax.net.ssl.trustStore=/var/oms/keystore/key.jks
-Djavax.net.ssl.trustStorePassword=secret4ever
-Xdebug
-Xrunjdwp:transport=dt_socket,server=y,address=8383,suspend=n
-Dlog4j2.disable.jmx=true
-DuiExtensibilityMode=true
```

Any other server specific settings can be made in the server.xml file.

6. Navigate to `/devtoolkit_docker/compose and run the following command:

```CMD
./om-compose.sh setup
```

7. Once installation is successful, following message will be displayed.

![image](https://user-images.githubusercontent.com/93929892/211502438-58a50395-f127-47ab-b28b-b1a622a2262b.png)


You can then access the call center and check: http://localhost:9080/isccsdev/isccs/login.do
