# GOGS REPOSITORIES FOR IMF2 APPLICATIONS
## Contents
1. **Logical Structures for IMF2 Repositories**
        THE RELEASE REPO	
        THE SNAPSHOT REPO
2. **The Security Model for IMF2 Repositories**
3. **Updating IMF2 Repositories**
        Option 1 – do it in the browser
        Option 2 – do it in a local directory and push via GIT command line
        Option 3 – Install a GUI GIT Client
4. **Converting IMF2 SVN homes and creating IMF2 Repositories**	
        Option 1: create a net new Repo 	
        Option 2: pull directly from SVN 	
5. **Jenkins Deployments of IMF2 sites**	
6. **For Contractors/Developers – FORK/PULL**	



-----
## 1. Repository Structures for IMF2 Repositories
For each IMF2 site, two repositories need to exist in GOGs under the IMF2 organization: https://gogs.data.gov.bc.ca/imf2. Each repo should include a description of the name of the IMF2 site, the relevant URLs, and business and technical contacts.

**THE RELEASE REPO**
**“(sitename)-release” = **
This private repository is restricted to internal staff, and has the build.xml files for TEST and PRODUCTION environments only. The repo should only consist of two folders and two files TEST/build.xml and PROD/build.xml.
    
**THE SNAPSHOT REPO**
**“(sitename)-snapshot” = **
This private repository is restricted to developers and internal staff, and contains the build.xml for DELIVERY and the IMF2 configuration files. The repo should consist of two main subfolders DELIVERY/build.xml and a folder (sitename) with the configuration files that will be copied to the IMF2 sites folder. 


-----
## 2. Security Model for Repositories
-	The RELEASE repo will only be accessible and modifiable by internal DataBC staff.
-	The SNAPSHOT repo will be accessible to developers and internal DataBC Staff. Depending on the project, and the number developers involved, there could be a designated lead developer that is granted the ability to commit to the master. Otherwise, DataBC Internal Staff may decide to approve commits and require developers to fork and pull their changes. For MORE INFO of the FORK/PULL process see - https://gogs.data.gov.bc.ca/DataBC/FAQ/wiki#what-are-the-git-workflow-can-i-push-my-changes-to-the-repo-directly
-	To change permissions Click on the Repo home and go to settings, Select on Collaboration in the left panel, and add new collaborators 
-	NOTE: all IMF2 repos must give read access to the account 'rel_mgr' (this gives read access for the service account used by jenkins)


-----
## 3.Updating IMF2 Repositories

**Option 1** – for minor file edits you can do it directly in the browser.  
**Option 2** – for more complex edits and additions you can modify it locally and push it to the master (if you have permission), or push it to a fork and request a pull. This can be done via GIT command line. For both option 2 and 3 you may need to set the environmental variable Set GIT_SSL_NO_VERIFY=true (to permit the SSL certificate for GOGS).
**Option 3** – Install GUI GIT Client https://git-scm.com/ 


-----
## 4. Converting IMF2 SVN homes and creating IMF2 Repositories
These actions are to be done by internal DataBC staff only.

**Option 1: create net new Repo** and manually put files there from SVN or elsewhere.
**FOR THE RELEASE REPO** 
 Create and go to a local directory like this - C:\sw_nt\git\(sitename)-release\
 Run the following repo initiation in the local directory :
    `git init`
 Copy the build.xml into TEST and PROD subdirectories (remove all SVN tags/history/trunk)
    `git add .`
    `git commit -m "first commit"`
    `git remote add origin https://gogs.data.gov.bc.ca/imf2/(sitename)-release.git (the RELEASE REPO created in section 1)`
    `git push -u origin master (enter your username/creds)`


 **FOR THE SNAPSHOT REPO**
 Create and go to a local directory like this - C:\sw_nt\git\(sitename)-snapshot\
 Run the following repo initiation in the local directory :
    `git init`
 Copy the SVN delivery\trunk\build.xml into a ‘DELIVERY’ subdirectory and copy the sites folder in svn (ie. \(sitename)\sites\trunk\(sitename) and remove all SVN tags/history/trunk)
    `git add .`
    `git commit -m "first commit"`
    `git remote add origin https://gogs.data.gov.bc.ca/imf2/(sitename)-snapshot.git (the SNAPSHOT REPO created in section 1)`
    `git push -u origin master`


 **Option 2: pull directly from SVN** 
 FOR THE SNAPSHOT REPO – this option will retain SVN history
 Create a folder local (\<create folder with repo name>)
    `Git svn init https://apps.bcgov/svn/(sitename)/`
    `Git svn fetch`
    `Git remote add origin https://gogs.data.gov.bc.ca/imf2/(sitename)-snapshot.git`
    `Git pull https://gogs.data.gov.bc.ca/imf2/(sitename)-snapshot.git master`
 Then clean up folders to match new REPO structure described in approach 1 section above.
    `Git add .`
    `Git commit –m “something” -a`
    `git push --set-upstream origin master`


-----
## 5. Jenkins Deployments of IMF2 sites
The current Jenkins deployment to support IMF2 applications in GIT is here: 
- https://cis-delivery.apps.gov.bc.ca/int/

In Jenkins All DLV/TST/PRD jobs for an IMF2 site will be located in the same folder. 
 (ie .. https://cis-delivery.apps.gov.bc.ca/int/job/habwiz/ contains three jobs one for each DLV/TST/PRD environment habwiz-dlv, habwiz-tst, habwiz-prd)

- New jobs can be cloned from habwiz-dlv, habwiz-tst, habwiz-prd.
- Delivery Jobs only pulls from SNAPSHOT repository.
- Test and Production Jobs need to pull from SNAPHOT and RELEASE.
- Jenkins Slaves for IMF2 is GEMMA/ALPHERATZ
- **JGIT** is the GIT Executable 
- Injected envs include: 
` GIT_SSL_NO_VERIFY=true`
` GENERAL.MAX_ALLOWABLE_TIME = 3`
` TEMP=E:\\sw_nt\\jenkins\\workspace\\temp`
` TMP=E:\\sw_nt\\jenkins\\workspace\\temp`

- **ANT** is used, with build.xml configuration files  to set environment specific variables:
- **ROBOCOPY** is used to copy to the appropriate IMF2 sites folders: 
-  **NOTE**: Exit Codes of ROBOCOPY interfer with JENKINS exit codes so this is added at end of batch:
`robocopy /MIR ".\sites\crpf" "E:\sw_nt\essentials\Default\REST Elements\Sites\crpf" /XD ".git"`
`REM NOTE ROBOCOPY exit status are messing with JENKINS exit status so HERE are exist status translated:`
`if errorlevel 16 echo ***FATAL ERROR*** & goto endError`
`if errorlevel 15 echo FAIL MISM XTRA COPY & goto endError`
`if errorlevel 14 echo FAIL MISM XTRA & goto endError`
`if errorlevel 13 echo FAIL MISM COPY & goto endError`
`if errorlevel 12 echo FAIL MISM & goto endError`
`if errorlevel 11 echo FAIL XTRA COPY & goto endError`
`if errorlevel 10 echo FAIL XTRA & goto endError`
`if errorlevel 9 echo FAIL COPY & goto endError`
`if errorlevel 8 echo FAIL & goto endError`
`if errorlevel 7 echo MISM XTRA COPY & goto endError`
`if errorlevel 6 echo MISM XTRA & goto endError`
`if errorlevel 5 echo MISM COPY & goto endError`
`if errorlevel 4 echo MISM & goto endError`
`if errorlevel 3 echo XTRA COPY & goto endSuccess`
`if errorlevel 2 echo XTRA & goto endSuccess`
`if errorlevel 1 echo COPY & goto endSuccess`
`if errorlevel 0 echo –no change– & goto endSuccess`
`:endSuccess`
`exit 0`
`:endError`
`exit 1`

-----
## 6. For Contractors/Developers – FORK/PULL

The SNAPSHOT repo will be accessible to developers and internal DataBC Staff. Depending on the project and the number developers involved there could be a designated lead developer that is granted the ability to commit to the master. Otherwise, DataBC Internal Staff may decide to approve commits and require developers to fork and pull their changes.
For MORE INFO of the FORK/PULL process see - https://gogs.data.gov.bc.ca/DataBC/FAQ/wiki#what-are-the-git-workflow-can-i-push-my-changes-to-the-repo-directly
 



