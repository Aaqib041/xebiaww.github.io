---
layout: post
header-img: img/default-blog-pic.jpg
---

# Error Code Handling in SVN Integration via SVN

Subversion is one of the most widely used Version Control System in IT industry. Many applications need to manage their project artifacts which can be easily addressed by use of a Version Control System. For integration with Subversion, I have used SVNKit ( [http://svnkit.com/](http://svnkit.com)) in my application. It supports all standard operations of Version Control System (like Check -in, Check-out, copy etc.) and works over SVN, HTTP, HTTPS, SSH and File protocols. While working on integration with Subversion (SVN) via SVNKit, I had encountered various exception and error code that needs to be handled in application for successful implementation of Version Control Operations. Some of those exceptions and error codes had stumped me and I was left wondering on how to move forward with this implementation. But a little bit of research and deeper dive to Subversion implementation helped me to bail myself out of this tricky situation. Below are the error codes that I had handled in my application while performing various Version Control Operations.  Though the below described SVN error codes are few in number and doesn't cover all, but it covers the common error codes that you'll encounter during the integration and it will also give you a fair idea on how to handle them. Please note that all these error codes are instances of class "org.tmatesoft.svn.core.SVNErrorCode" which represents predefined error with description. It can be retrieved from instance of class "org.tmatesoft.svn.core.SVNException" in following manner: 
    
    
     SVNErrorCode errorCode = ((SVNException) svne).getErrorMessage() .getErrorCode();  
     if (errorCode == SVNErrorCode.WC_LOCKED) { //handle exception  
     ...  
     ...  
     } else if (errorCode == SVNErrorCode.WC_OBSTRUCTED_UPDATE) {  
     ...  
     ... }  
     ... 
     ...  
    

  1. **WC_LOCKED**: You'll get this error code, when you will attempt to lock an already locked directory. It can be resolved by performing cleanup operation on working copy via "org.tmatesoft.svn.core.wc.SVNClientManager" object. It will remove stale locks and get your working copy into a usable state again.                                                                                                 
    
         svnClientManager.getWCClient().doCleanup(workingCopyPath, deleteWCProperties)  
    

  2. **CLIENT_MODIFIED**: You'll get this error code, when you try to attempt a restricted operation on a modified resource. I got this error code while performing 'delete' operation on Version Control Project artifact. Since my application always performed hard delete, I just caught this exception and performed a forceful delete on Version Control Project artifact.
  

  3. **WC_OBSTRUCTED_UPDATE:** You'll get this error code when update operation is obstructed by SVN Server. This usually happens when your local working copy is associated with different SVN connection URL while you are trying to perform Version Control operation (like Check- out) with different SVN Connection URL. A typical use case where you can receive this error is when IP or hostname of the machine where your SVN Server is located changes. In this case, your local working copy will be associated with SVN connection URL having old IP or hostname while you are performing Version Control operation with new IP or hostname.It can be resolved by re-locating your working copy to new SVN connection URL via below method: 
    
         svnClientManager.getUpdateClient()  
     .doRelocate(versionControlProjectFolder, oldSVNURL,newSVNURL, true);
    

You can retrieve old SVN connection URL from your working copy in following manner: 
    
        /*  
     * Revision is taken as UNDEFINED as it throws exception for HEAD * revision if WC URL changes  
     */
    SVNInfo wcInfo = svnClientManager.getWCClient().doInfo( versionControlProjectFolder, SVNRevision.UNDEFINED);
    
    SVNURL oldSVNURL = wcInfo.getURL();  
    

  4. **WC_FOUND_CONFLICT or FS_TXN_OUT_OF_DATE:** You'll get this error code when there is a conflict in the working copy that obstructs the current operation. A typical use case when you can receive this error is when you try to commit the modification made to any lower revision of Version Control Project artifact. You may argue the validity of this use case, but believe me, many developers and programmers have to deal with this use case where there is a need to make some modifications to lower revision of artifact and then commit to Version Control System at 'Head' (or top) revision.It can be resolved by calling an update operation followed by performing an automatic conflict resolution on working copy path. Here is the code snippet that does this job for you: 
    
           svnClientManager.getUpdateClient().doUpdate(checkoutDestpath,SVNRevision.create(-1), SVNDepth.INFINITY,allowUnversionedObstructions, depthIsSticky); 
     
     try {  
     /*  
     * SVNConflictChoice is kept as MINE_FULL to resolve the * conflict which occurs when we check-in an artifact after * modifying it on lower revision  
     */  
     svnClientManager.getWCClient().doResolve(checkoutDestpath, SVNDepth.INFINITY, SVNConflictChoice.MINE_FULL);  
     } catch (SVNException e) {  
     final SVNErrorCode resolveErrorCode = svne.getErrorMessage().getErrorCode();  
     if (resolveErrorCode == SVNErrorCode.WC_FOUND_CONFLICT) { 
     ....
     .... 
     } }  
     /*  
     * SVNConflictChoice is kept as MERGED to resolve the  
     * conflict which occurs when we call version control  
     * delete  
     */  
     svnClientManager.getWCClient().doResolve(checkoutDestpath, SVNDepth.INFINITY, SVNConflictChoice.MERGED);