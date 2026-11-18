The audit that produces these events is set up in [[Share and Audit a Folder (Windows Server)]].

## Wazuh agent configuration

In `C:\Program Files (x86)\ossec-agent\ossec.conf`, or from your agent manager (**View → View configuration**), make sure you have this configuration in the file:

```xml
<localfile>

  <location>Security</location>

  <log_format>eventchannel</log_format>

  <query>Event/System[EventID != 5145 and EventID != 5156 and EventID != 5447 and

    EventID != 4656 and EventID != 4658 and EventID != 4660 and

    EventID != 4670 and EventID != 4690 and EventID != 4703 and EventID != 4907 and

    EventID != 5152 and EventID != 5157]</query>

</localfile>
```

## Wazuh manager rules

On your Wazuh manager, create a rule file to audit these rules. The variable `CriticalFolders` contains the folders for which the rules trigger, for example `informatique`. You can add several folders by replacing the `C:\\\\Critical_Folder2` part.

```xml
<var name="CriticalFolders">C:\\\\informatique|C:\\\\Critical_Folder2</var>

<group name="windows, windows_security,">

  <rule id="100016" level="0">

     <if_sid>60103</if_sid>

     <field name="win.system.eventID">^4663$</field>

     <field name="win.eventdata.objectName">$CriticalFolders</field>

     <description>Object accès information into critical folders</description>     

     <options>no_full_log</options>

  </rule>

  <rule id="100017" level="10">

     <if_sid>100016</if_sid>

     <time>8am - 8pm</time>

     <description>$(win.eventdata.subjectUserName) accès au dossier (lire) $(win.eventdata.objectName) </description>    

     <options>no_full_log</options>

  </rule>

  

  <rule id="100018" level="12">

  <if_sid>100016</if_sid>

  <field name="win.eventdata.accessMask">^0x2$|^0x40000$</field>

  <!-- 

    0x2     = WRITE_DATA (file modification)  

    0x40000 = CHANGE_PERMISSIONS  

  -->

  <description>l'usager $(win.eventdata.subjectUserName) a fait une modification ($(win.eventdata.accessMask)) sur $(win.eventdata.objectName)</description>

  <options>no_full_log</options>

</rule>

  

 <rule id="100019" level="12">

  <if_sid>60103</if_sid>

  <field name="win.system.eventID">^4659$</field>

  <field name="win.eventdata.objectName">$CriticalFolders</field>

  <description>CRITICAL:l'usager $(win.eventdata.subjectUserName) explicitement supprime $(win.eventdata.objectName)</description>

  <options>no_full_log</options>

</rule>

</group>
```

The rule descriptions are in French. In English:

- 100016: object access information in critical folders (level 0)
- 100017: `<user>` accessed folder (read) `<object>`, between 8am and 8pm
- 100018: the user `<user>` made a modification (`<access mask>`) on `<object>`
- 100019: CRITICAL: the user `<user>` explicitly deleted `<object>`

## Test

Run your tests from a user account of the Windows Server: delete, modify and add.

![[detect-critical-folder-access--fig-01--delete-modify-add-test.png]]
*result of the delete, modify and add test*

The alerts are for the user `waelgs` on the agent `Server`, file `C:\informatique\infos.txt`: rule 100019 (level 12) "CRITICAL: waelgs explicitly DELETED …", rule 100017 (level 10) "waelgs accessed … folder out of office hours.", rule 100018 (level 12) "l'usager waelgs a fait un modification (0x2) sur …", and rule 60642 "Software protection service scheduled successfully."

