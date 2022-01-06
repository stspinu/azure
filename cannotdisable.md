If the extension gets stuck after you try to delete it (ex: from the Azure Portal) and you see the bell icon constantly loading and the extension status being Transitioning, then you are most likely running into this scenario.

The error message on the Activity Log of the VM and in the extension.log on the server might be:


**<font color="red">Disable, failed, 1, OMSAgent service control script /opt/microsoft/omsagent/bin/service_control doesnot exist. Disable cannot be called before install.</font>**

![image](https://user-images.githubusercontent.com/46924453/148441230-e7a9fdf6-b323-4359-9ebb-c41d413f2d6a.png)


The new purge script requires you to un-install the extension first, but when attempting to remove the extension, you run into this code path

OMSAgentServiceScript = '/opt/microsoft/omsagent/bin/service_control'

And since the path is actually there on the system (because the omsagent still exists on the machine, even though it is most likely a broken agent), you get this error.




Here is what you can do:

Remove all the files with the format Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux* from /var/lib/waagent/ directory:

Find these files:

ls /var/lib/waagent/ | grep -i "omsagent"



Go to that directory and remove the files/folders (usually 3) one by one:

cd /var/lib/waagent/

rm -rf  Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux-1.13.40
rm -rf  Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux__1.13.40.zip
rm -rf  Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux.1.manifest.xml


