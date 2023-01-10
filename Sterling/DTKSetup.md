**DTK Installation**

1. Download the latest DTK toolkit (devtoolkit_docker_10.0.2209.0.tar) from IBM selfserve tool https://selfservice.oms.supply-chain.ibm.com/selfserve.

2. Extract the tar file. The exploded folder will contain the following sub-folders and files.

![image](https://user-images.githubusercontent.com/93929892/211470566-dbf2d5df-4d12-4142-9302-c0b978e5e380.png)

3. Navigate to compose directory. Make a copy of om-compose.properties.sample to om-compose.properties. Add the following configuration:

'''PROP
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
'''
