# mobile_string_translations

This repository holds the archived string tables for the mobile apps (currently just iOS), so that they can be uploaded to Fastly Servers and downloaded dynamically at runtime by the app.

### Dynamic String Handling

A feature exists in the iOS app [iOS GitHub Repo](https://github.com/FootLocker-CE/FootLockerGlobal-iOS) that allows string tables to be downloaded to the app dynamically at run time. These string tables need to be archived into zip files and uploaded to this repository.
From there, they will be picked up and deployed via Fastly to endpoints that the app can hit. The `DynamicStringsManager` class in the iOS app is responsible for handling the downloading and unarchiving of the new string tables, as well as the selection of the proper localized string at runtime. 

#### string-archiver utility

This is a command line utility that lives in the `scripts` folder under the project root. Its purpose is to create an archive file for each banner that can be added to the repository mentioned above. In order to create the archives for all the banners, you can run
```
strings-archiver --all --appver "x.y.z"
```
from the command line. Where the --appver parameter is the version number of the current live app or app to be published. Any apps that have this version number or *earlier* will be able to use the string archives created. If an app has a *higher* version number than is specified, then it will ignore the archives. This is done to prevent the case where a newer version of the app would pick up an older string archive.

This will create a folder named `archived_strings` that will contain an archive for each banner using the banner id as the filename:
```
fl = FootLocker
ch = Champs
kfl = Kids FootLocker
flca = FootLocker Canada
fleu = FootLocker Europe
```

Take these archives and commit them to this GitHub repository. A job will run that will pick up the latest commits and send them to the Fastly servers.
