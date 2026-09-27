---
title: "SOB | WEB & APP | Fraud Detection | Investigation | BE-1868"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/48270442747/SOB+WEB+APP+Fraud+Detection+Investigation+BE-1868
space: "Arrow"
topic: programming
relevance: 0.703
depth: 2.41
updated: 2025-01-20
attachments: 18
tags:
  - confluence
  - programming
  - space/arrow
---

# SOB | WEB & APP | Fraud Detection | Investigation | BE-1868

> [!info] Imported from Confluence
> Space **Arrow** · updated 2025-01-20 · [open original](https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/48270442747/SOB+WEB+APP+Fraud+Detection+Investigation+BE-1868)
> Relevance 0.703 · topic `programming`

# **Option 1: Immediate Fraud Notification** 

This option takes less effort and can implement normally.  
  
After an Officer completed reviewing a dossier, Fidentity will callback to Ivy an API to notify the review is finish:  

<span rel="nofollow">**https://\[IVY_DOMAIN\]/ap/ga/ff/mobile/api/dossiers/\[dossierId\]/identifications/callback?requestId=\[requestId\]**</span>

- **IVY_DOMAIN**: Domain that FinForm using to deploy Ivy and other services

- **dossierId:** The dossier’s id that Officer reviewed

- **requestId**: The request’s id of dossier above.

After Fidentity notify when review finish, Ivy will call an API to Fidentity to get status of that dossier.


![[48270442747-image-20250120-103304.png]]



We can check that if the status whether or not the status is **SUSPICION_OF_FRAUD** then we can immediately send a notification email.  

Conclusion:

- Take less effort to do.

- No need to modify Agent Review application only need to implement in Ivy.

- Easy to test

# **Option 2 + 3: Delayed Notification and Two-Stage Notification**

**Note:** Because the approach for both case will be the same. I will merge this into one section

Same as option one, Fidentity will notify Ivy when finishing review and with that we can create a logic code to process after the NOK_FRAUD is present.

Ivy provide us a way to create a timer and can also cancel that timer.  
Example of a timer and canceling a timer:  


![[48270442747-image-20250120-105154.png]]




![[48270442747-image-20250120-105344.png]]



: The starting point of an Signal (using name for signal)


![[48270442747-image-20250120-105443.png]]



: Including script for business, logic or modify code


![[48270442747-image-20250120-105701.png]]



: The delayed function (Or timeout) so that the process can pause before continue processing


![[48270442747-image-20250120-105831.png]]



: Error start


![[48270442747-image-20250120-105807.png]]



: Process end

**To start a signal for timer:**  
First we must create a signal so that we can call to that signal (add a name for it).  


![[48270442747-image-20250120-111937.png]]



  
**Note:**  
We must tick the checkbox when creating process to make it assign to the dossier we are currently working on.


![[48270442747-image-20250120-112107.png]]



To call to that signal, we use these lines of code.


![[48270442747-image-20250120-110148.png]]



This will create a trigger to run a process also send the data to that signal.  
  
After converting the signal data (Because when we trigger a process and also send data to that singal, it will be as JSON string so we have to convert to Java data), it will go to next Timer or blocker block:  


![[48270442747-image-20250120-110721.png]]



This block will stop the current process until the time is expired. To configure the time, double click that block.  


![[48270442747-image-20250120-111026.png]]



**NOTE:**  
This block use an empty dialog to stop the process (this dialog has no function or logic).


![[48270442747-image-20250120-111201.png]]



**To cancel a running signal timer:**

We create a new signal in the same process mod. and put it inside the Timer block (IMPORTANT)


![[48270442747-image-20250120-111532.png]]



This will cancel the timer and process directly to “process end”.  
We just need to call to the new process just created with the this line of code (change the name of process).


![[48270442747-image-20250120-111738.png]]



Conclusion:

- Take more effort to do.

- No need to modify Agent Review application only need to implement in Ivy.

- Hard to test.
