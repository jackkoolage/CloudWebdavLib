# CloudWebdavLib
**a .NET WebDAV Client Library for Cloud Providers**

# Features:
* Assemblies for .NET 4.5.2
* Just one external reference (Newtonsoft.Json)
* Easy installation using NuGet
* Upload/Download tracking support
* Proxy Support
* Upload/Download cancellation support

# List of functions:
> * Quota
> * Copy
> * Move
> * Delete
> * List
> * Rename
> * CreateNewFolder
> * Exists
> * Upload
> * DownloadFile
> * DownloadFileAsStream
> * DownloadFileAsString


## Supported Cloud Storage Providers
> * 4Shared.com
> * AllSync.com
> * Box.com
> * CloudMe.com
> * DriveOnWeb.de
> * Files.fm
> * HiDrive.com
> * Koofr.net
> * MagentaCLOUD.de
> * MyDrive.ch
> * oCloud.de
> * OpenDrive.com
> * OwnDrive.com
> * pCloud.com
> * PicturePush.com
> * PowerFolder.com
> * SmartFile.com
> * StorageMadeEasy.com
> * SwissDisk.com
> * TeraCloud.jp
> * Woelkli.com
> * Zaclys.com
> * LetsUpload.co
> * MailBox.org

# Code simple:
```vb
Dim root As New CloudWebdavLib.JSON.Node
Dim client As CloudWebdavLib.IClient = New CloudWebdavLib.WClient(CloudWebdavLib.CloudDav.ProvidersNameEnum.pCloud, "user", "pass", root, Nothing)

''OR with proxy
''set proxy and connection options
Dim conn As New CloudWebdavLib.ConnectionSettings With {.CloseConnection = True, .TimeOut = TimeSpan.FromMinutes(30), .Proxy = New CloudWebdavLib.ProxyConfig With {.SetProxy = True, .ProxyIP = "127.0.0.1", .ProxyPort = 8888, .ProxyUsername = "user", .ProxyPassword = "pass"}}
Dim client2 As CloudWebdavLib.IClient = New CloudWebdavLib.WClient(CloudWebdavLib.CloudDav.ProvidersNameEnum.pCloud, "user", "pass", root, conn)

''get storage usage if available
Await client.Quota

''list files/folders in root node
Await client.List(root, 1)

''create TEMP nodes to use it in the examples
Dim fileNode As New CloudWebdavLib.JSON.Node
Dim folderNode As New CloudWebdavLib.JSON.Node

Await client.Copy(fileNode, folderNode)
Await client.Move(fileNode, folderNode, Nothing, False)
Await client.Rename(fileNode, "new_name.rar")
Await client.CreateNewFolder(folderNode, "folder name")
Await client.Delete(fileNode)
Await client.Exists(folderNode.ShortPath)

Dim cts As New Threading.CancellationTokenSource()
Dim _ReportCls As New Progress(Of CloudWebdavLib.ReportStatus)(Sub(ReportClass As CloudWebdavLib.ReportStatus) Console.WriteLine(String.Format("{0} - {1}% - {2}", String.Format("{0}/{1}", (ReportClass.BytesTransferred), (ReportClass.TotalBytes)), CInt(ReportClass.ProgressPercentage), ReportClass.TextStatus)))

Await client.Upload("c:\\VIDO.mp4", CloudWebdavLib.utilities.UploadTypes.FilePath, folderNode, "VIDO.mp4", _ReportCls, cts.Token)
Await client.DownloadFile(fileNode, "c:\\downloads\VIDO.mp4", _ReportCls, cts.Token)
Await client.DownloadFileAsStream(fileNode, _ReportCls, cts.Token)
Await client.DownloadFileAsString(fileNode, _ReportCls, cts.Token)
```
