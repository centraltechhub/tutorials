
**ydkprefs.xml**

1. Rename <INSTALL_DIR>/resources/ydkresources/ydkprefs.xml.sample file to ydkprefs.xml

2. Update the content with the necessary CDT information. Working sample for CDT export to XML below:

```XML
<preferences>
    <configsynch>
            <Settings CustomEntityClass="" MaxChangesToDisplay="15000" ReportsDir="/home/admin/App/CDT/CDT_IMP">
            <AuditDeployment ValidateLockid="Y" ValidateOldValues="Y" ValidateRecordExistsBeforeDelete="Y"/>
        </Settings>
        <SourceDatabases>
                <Database Name="OMSNPR" className="org.postgresql.Driver" dbType="postgresql" folder="" jdbcURL="jdbc:postgresql://pgdb:5432/OMSNPR" schema="postgres" user="postgres"/>
        </SourceDatabases>
        <TargetDatabases>
                <Database Name="CDTXML" className="" dbType="xml" folder="/home/admin/App/CDT/CDT_IMP" jdbcURL="" schema="" user="admin"/>
        </TargetDatabases>
        <SourceTargetPrefs>
          <SourceTargetPrefs>
                <SourceTargetPair SourceDatabase="OMSNPR" TargetDatabase="OMSNPR">
                    <Transformations>
                    </Transformations>
                    <Ignore>
                    </Ignore>
                    <AppendOnly>
                    </AppendOnly>
            </SourceTargetPair>
           </SourceTargetPrefs> 
        </SourceTargetPrefs>
    </configsynch>
</preferences>
```

Working sample for CDT import to XML below:

```XML
<preferences>
    <configsynch>
                <Settings CustomEntityClass="" MaxChangesToDisplay="15000" ReportsDir="/Users/hussamoa/Techhub/DTK/runtime/CDT_IMP">
                <AuditDeployment ValidateLockid="N" ValidateOldValues="Y" ValidateRecordExistsBeforeDelete="Y"/>
            </Settings>
        <SourceDatabases>
            <Database Name="CDTXML" className="" dbType="xml" folder="/Users/hussamoa/Techhub/DTK/runtime/CDT_IMP" jdbcURL="" schema="" user="admin"/>
            </SourceDatabases>
        <TargetDatabases>
            <Database Name="OMDB" dbType="db2" className="com.ibm.db2.jcc.DB2Driver" folder="" jdbcURL="jdbc:db2://localhost:50000/OMDB" schema="OMDB" user="db2inst1"/>
            </TargetDatabases>
            <SourceTargetPrefs>
              <SourceTargetPrefs>
                    <SourceTargetPair SourceDatabase="CDTXML" TargetDatabase="OMDB">
                        <Transformations>
                        </Transformations>
                        <Ignore>								
                        </Ignore> 
                        <AppendOnly>
                        </AppendOnly>
                </SourceTargetPair>
               </SourceTargetPrefs> 
            </SourceTargetPrefs>
        </configsynch>
    </preferences>
```

3. Update cdtshell.sh.in in the runrime bin directory.

```
SOURCE_DB=CDTXML
export SOURCE_DB

SOURCE_PASSWORD=
export SOURCE_PASSWORD

TARGET_DB=OMSNPR
export TARGET_DB

TARGET_PASSWORD=diet4coke
export TARGET_PASSWORD

${JAVA} ${HEAP_FLAGS} -classpath &JAR_DIR;/bootstrapper.jar:&INSTALL_DIR;\resources\ydkresources **-Xms4096m -Xmx4096m** -Dvendor=shell -DvendorFile=&PROP_DIR;/servers.properties -DUseEntitiesFromClasspath=Y -DDISABLE_DS_EXTENSIONS=Y -DDISABLE_EXTENSIONS=Y -DYFS_HOME=&HOME_DIR; com.sterlingcommerce.woodstock.noapp.NoAppLoader -class com.yantra.tools.ydk.config.ConfigDeployMain -p &HOME_DIR;/resources/ydkresources -f &PROP_DIR;/dynamicclasspath.cfg -invokeargs ${CDT_ARGS} "$@" 2>&1 | tee &LOG_DIR;/cdtshell.${dstamp}.log
exit ${PIPESTATUS[0]}
```
