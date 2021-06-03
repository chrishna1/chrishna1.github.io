---
title: "Use rclone with OneDrive(Business plan) without Business API access"
date: 2021-06-03T10:16:53+05:30
draft: false
tags: ["automate-the-boring-stuff", "cli", "rclone"]
showToc: false
TocOpen: true
---

Hi there  :wave:

Let's say you have to share a report that you're generating programatically with a consultant in your company every week. Your general approach might be to upload it to your organization's document/file storage platform(e.g OneDrive) and share it. If you've to do this multiple times, it's boring to perform the same steps over and over again! 

What about automating this process? As soon as your report is generated you can run a command that will copy your file present in local to OneDrive server. That would be awesome, right? To achieve that you can use a powerful tool - `rclone`.

Today, I'll describe to how to configure `rclone` with OneDrive(Business plan). `rclone` doesn't needs an introduction but in case you're not familiar with it -- it's the most popular open-source cli tool to interact with your cloud storage providers. It supports more than 40 cloud storage providers out of the box.

If you have business api access provided by your admin then it's pretty straightforward to configure. But if you don't have the api access then you'll get following error while authentication. 


![rclone_admin](https://user-images.githubusercontent.com/26048398/120586970-1befe100-c452-11eb-9096-520071dce115.png)


It is because official API is protected and can only be enabled by administrator. But don't worry. We have an alternative.

We can use webdav protocol supported by SharePoint and also by rclone luckily.

Your configuration process should look like

```
~ rclone config
No remotes found - make a new one
n) New remote
s) Set configuration password
q) Quit config
n/s/q> n
name> work_onedrive
Type of storage to configure.
Enter a string value. Press Enter for the default ("").
Choose a number from below, or type in your own value
...
36 / Union merges the contents of several upstream fs
   \ "union"
37 / Webdav
   \ "webdav"
38 / Yandex Disk
   \ "yandex"
...
Storage> 37
** See help for webdav backend at: https://rclone.org/webdav/ **

URL of http host to connect to
Enter a string value. Press Enter for the default ("").
Choose a number from below, or type in your own value
 1 / Connect to example.com
   \ "https://example.com"
url> https://company_name-my.sharepoint.com/personal/your_username/Documents/
Name of the Webdav site/service/software you are using
Enter a string value. Press Enter for the default ("").
Choose a number from below, or type in your own value
 1 / Nextcloud
   \ "nextcloud"
 2 / Owncloud
   \ "owncloud"
 3 / Sharepoint Online, authenticated by Microsoft account.
   \ "sharepoint"
 4 / Sharepoint with NTLM authentication. Usually self-hosted or on-premises.
   \ "sharepoint-ntlm"
 5 / Other site/service or software
   \ "other"
vendor> 3
User name. In case NTLM authentication is used, the username should be in the format 'Domain\User'.
Enter a string value. Press Enter for the default ("").
user> username@company_name.com
Password.
y) Yes type in my own password
g) Generate random password
n) No leave this optional password blank (default)
y/g/n> y
Enter the password:
password:
Confirm the password:
password:
Bearer token instead of user/pass (e.g. a Macaroon)
Enter a string value. Press Enter for the default ("").
bearer_token> 
Edit advanced config? (y/n)
y) Yes
n) No (default)
y/n> 
Remote config
--------------------
[work_onedrive]
type = webdav
url = https://company_name-my.sharepoint.com/personal/your_username/Documents/
vendor = sharepoint
user = username@company_name.com
pass = *** ENCRYPTED ***
--------------------
y) Yes this is OK (default)
e) Edit this remote
d) Delete this remote
y/e/d> y
Current remotes:

Name                 Type
====                 ====
work_onedrive     webdav

e) Edit existing remote
n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
e/n/d/r/c/s/q> q
```
Once configuration is completed successfully, you can run following command to ensure your remote is available.

```
~ rclone listremotes
work_onedrive:
```

If you can see your remote, now, it's just a matter of one command to copy your local file to file storage platform. Here, I'm copying a `test.txt` in my local system to `Attachments/` folder on remote.

```
~ rclone copy test.txt work_onedrive:/Attachments/
```

You can try few other commands as well.
```
~ rclone tree work_onedrive:
/
├── example1.xlsx
├── Attachments
│   └── example2.xlsx
├── example3.xlsx
├── Forms
│   ├── example4.aspx
│   ├── example5.aspx

9 directories, 52 files

~ rclone lsd work_onedrive:
    -1 2021-02-22 21:45:50        -1 Attachments
    -1 2019-04-13 14:18:07        -1 Forms
    -1 2021-04-14 15:24:31        -1 Microsoft Teams Chat Files
    -1 2020-04-01 11:25:28        -1 Microsoft Teams Data
    -1 2021-05-24 15:50:23        -1 Notebooks
```

Now, you can use this not only for sharing report 😆 but also automate your backing up your files, storing your archives, etc. 


Have fun, Keep hacking!