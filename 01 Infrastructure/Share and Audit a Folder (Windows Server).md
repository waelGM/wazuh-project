## Share a folder

1. To share a document in Windows Server, create a folder or share one that already exists on your system.
2. Click **Permissions**. Remove the group **Everyone** and add a specific group that can enter, and make changes or create files or subfolders. The share is `informatique` (path `\\SERVER\informatique`) and the group `tech` (`GESTION\tech`) is allowed Full control, Modify and Read:

![[share-and-audit-a-folder-windows-server--extra-11--share-permissions.png]]
*share permissions for the group `tech` (the dialog is in French)*

### Test the share

1. Go to a machine joined to the domain and open File Explorer.
2. Right-click **Network** in Quick access and click **Map network drive**.
3. Choose a drive letter.
4. In the **Folder** field, enter the path to your folder: `\\<name of your Windows Server computer or IP address of your file server>\<shared folder name>`.
5. Click **Finish**.

You should see the drive like this:

![[share-and-audit-a-folder-windows-server--fig-01--mapped-drive.png]]
*the mapped network drive*

## Audit the shared folder

1. In Windows Server, click **Tools**, then **Group Policy Management**.
2. An organizational unit called `informatique` contains the group `tech`, the only group allowed into the `informatique` folder.
3. Right-click the OU and click **Create a GPO in this domain**. Name it `Audit_Dossier_Informatique`, or as you like.
4. Right-click the new GPO and click **Edit**.
5. Go to **Computer Configuration → Policies → Windows Settings → Security Settings → Advanced Audit Policy Configuration → Audit Policies → Object Access → Audit File System**.
6. Tick **Configure the following audit events** and select **Success** and **Failure**. Click **Apply**, then **OK**.

![[share-and-audit-a-folder-windows-server--extra-18--audit-policy-file-system.png]]
*Group Policy editor: Object Access, Audit File System set to Success and Failure (the editor is in French)*
7. Open a command prompt and run:

```powershell
gpupdate /force
```

### Apply the audit to the file

1. Right-click the file you want to audit and choose **Properties**.
2. Go to **Security → Advanced → Auditing → Add**.
3. Click **Select a principal** (the group you want, or Everyone). Set **Type** to **All**.
4. Click **OK**, **Apply**, then **OK**.

The auditing entry has the principal `tech` (`GESTION\tech`), **Type** set to All, applied to "This folder, subfolders and files":

![[share-and-audit-a-folder-windows-server--extra-30--audit-entry.png]]
*auditing entry for the folder `informatique`*

To see how to detect the events in Wazuh, see [[Detect Critical Folder Access]].
