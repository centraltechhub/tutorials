
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
            <Settings CustomEntityClass="" MaxChangesToDisplay="15000" ReportsDir="/home/admin/App/CDT/CDT_IMP">
            <AuditDeployment ValidateLockid="N" ValidateOldValues="Y" ValidateRecordExistsBeforeDelete="Y"/>
        </Settings>
	<SourceDatabases>
		<Database Name="CDTXML" className="" dbType="xml" folder="/home/admin/App/CDT/CDT_IMP" jdbcURL="" schema="" user="admin"/>
        </SourceDatabases>
	<TargetDatabases>
		<Database Name="OMSNPR" className="org.postgresql.Driver" dbType="postgresql" folder="" jdbcURL="jdbc:postgresql://pgdb:5432/OMSNPR" schema="postgres" user="postgres"/>
        </TargetDatabases>
        <SourceTargetPrefs>
          <SourceTargetPrefs>
                <SourceTargetPair SourceDatabase="CDTXML" TargetDatabase="OMSNPR">
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
