---
title: "Routing tool | Database replacement | NodeJS - Investigation"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/48139206657/Routing+tool+Database+replacement+NodeJS+-+Investigation
space: "Arrow"
topic: programming
relevance: 0.775
depth: 2.6
updated: 2024-11-07
attachments: 6
tags:
  - confluence
  - programming
  - space/arrow
---

# Routing tool | Database replacement | NodeJS - Investigation

> [!info] Imported from Confluence
> Space **Arrow** · updated 2024-11-07 · [open original](https://axonivy.atlassian.net/wiki/spaces/Arrow/pages/48139206657/Routing+tool+Database+replacement+NodeJS+-+Investigation)
> Relevance 0.775 · topic `programming`

<div class="toc-macro client-side-toc-macro conf-macro output-block" cssliststyle="none" hasbody="false" headerelements="H1,H2,H3,H4,H5,H6" macro-id="61929221-f014-4b60-a6a0-7652bf8cfe23" macro-name="toc" numberedoutline="false" structure="list">

</div>

## 1. Challenges

### 1.1. Database migration

#### 1.1.1. node-pg-migrate

**Refer:**

- The library <a href="https://www.npmjs.com/package/node-pg-migrate" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.npmjs.com/package/node-pg-migrate</a>

- The guideline <a href="https://synvinkel.org/notes/node-postgres-migrations" class="external-link" data-card-appearance="inline" rel="nofollow">https://synvinkel.org/notes/node-postgres-migrations</a>


![[48139206657-image-20241106-032446.png]]



**Pros**:

- Frequently updated (last month)

**Cons**:

- (Seam like) Must write changesets in JS code and doesn’t support SQL changesets

- Doesn’t run automatically at the startup time but need to run `npm run migrate up` separately. Can utilize Docker for that but Routing Tool on Docker yet


![[48139206657-image-20241106-033124.png]]



#### 1.2.2. postgres-migrations

**Refer:**

- The library <a href="https://www.npmjs.com/package/postgres-migrations" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.npmjs.com/package/postgres-migrations</a>

**Example usage:**


![[48139206657-image-20241106-034231.png]]



**Pros:**

- Support changesets written in both `SQL` and `Javascript`

- Can run at service startup out of the box

**Cons:**

- Last update 3 years ago

- Less popular

#### 1.1.3. db-migrate

**Refer:**

- The library <a href="https://db-migrate.readthedocs.io/" class="external-link" data-card-appearance="inline" rel="nofollow">https://db-migrate.readthedocs.io/</a>

**Example usage:**


![[48139206657-image-20241106-035523.png]]



**Pros:**

- Frequently update (4 months ago)

- Support changesets written in both `SQL` and `Javascript`

**Cons:**

- Kind of new (published 1 year ago)

- The same as 1.1.1, must run `db-migrate-up` separately.

### 1.2. Database ORM Framework

- Similar to Hibernate in Quarkus stack

- Sequelize <a href="https://sequelize.org/" class="external-link" data-card-appearance="inline" rel="nofollow">https://sequelize.org/</a>

- Prisma <a href="https://www.prisma.io/docs" class="external-link" data-card-appearance="inline" rel="nofollow">https://www.prisma.io/docs</a>

## 2. Data models

<div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="7d84fd49-0299-4981-adcb-03bcb5a97d47" macro-name="code" style="border-width: 1px;">

<div class="codeContent panelContent pdl">

``` syntaxhighlighter-pre
chat                    1pts
alert                   2pts
chatalert               2pts
alertnotification       1pts
chatnotification        1pts
callRecom               1pts
chatMb                  1pts
coComment               1pts
cobIdScanImage          1pts
cobIdScanMeta           1pts
cobIdScanOrig           1pts
holiday                 1pts
servicetime             2pts
model                   1pts
kyccheck                1pts
link                    1pts
livereport              0pts => do not have schema
message                 1pts
operationalplan         3pts => can refactor its fields to seperated tables: group, priority, supervisor
supervisorRequest       1pts
supervisorNotification  1pts  
taskInfo                1pts
userAction              1pts
user                    3pts => can refactor to seperated tables: priority
```

</div>

</div>

**Total: 30pts**

1.  **server.js (2 APIs)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="8a5f2a8a-b025-4801-b0bf-4d4555c4d674" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    app.get('/api/ping', (req, res) => {
        res.status(200).json({ message: 'Service is up and running.'});
        }
    );
    => 0pts

    app.get('/api/pingSession', (req, res) => {
        if (req.session.user && req.session.isAuthenticated) {
            res.status(200).json({ isAuth: true, username: req.session.user.username });        
        } else {
            res.status(200).json({ isAuth: false, username: '' });
        }    
    }
    => 0pts
    ```

    </div>

    </div>

    **Total: 0pts**

2.  **alerts (20 APIs)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="61359829-a417-417e-b563-9b9267fc5f26" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/getMyAlerts').get(controller.getMyAlerts); 4pts
    router.route('/getMyActiveAlerts').get(controller.getMyActiveAlerts); 6pts
    router.route('/getMyActiveChatAlerts').get(controller.getMyActiveChatAlerts); 6pts
    router.route('/getStatAlertsByCobStatus').get(controller.getStatisticsAlertyPerCobStatus); 3pts
    router.route('/all').get(controller.getAllAlerts); 4pts
    router.route('/allActive').get(controller.getAllActiveAlerts); 2pts
    router.route('/allActiveChats').get(controller.getAllActiveChatAlerts); 3pts
    router.route('/getByCobId').post(controller.getByCobId); 3pts
    router.route('/settleChatAlert').post(controller.settleChatAlert); 3pts
    router.route('/closeChatAlert').post(controller.closeChatAlert); 3pts
    router.route('/closeTaskAlert').post(controller.closeTaskAlert); 3pts
    router.route('/addAlertNotification').post(controller.addAlertNotification); 2pts
    router.route('/addChatNotification').post(controller.addChatNotification); 2pts
    router.route('/forwardAlert').post(controller.forwardAlert); 8pts
    router.route('/forwardChatAlert').post(controller.forwardChatAlert); 8pts
    router.route('/search').post(controller.search); 10pts
    router.route('/updateCurrentAssignee').post(controller.updateCurrentAssignee); 8pts
    router.route('/getOutOfServiceAlerts').get(controller.getOutOfServiceAlerts);  3pts
    router.route('/getOutOfServiceChatAlerts').get(controller.getOutOfServiceChatAlerts); 3pts
    router.route('/updateChatProcessingStatus').post(controller.updateChatProcessingStatus); 3pts
    ```

    </div>

    </div>

    **Total: 87pts**  

3.  **authenticate (1 API)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b85ace85-b16e-4423-acbd-a6b27c9b2f6d" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/').post(controller.postAuthenticate); 5pts
    ```

    </div>

    </div>

    **Total: 5pts**  

4.  **callRecoms (1 API)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9ece14e5-17a2-42fe-916c-8ec916612e4d" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/getCallRecomsByCobId').post(controller.getCallRecomsByCobId); 4pts
    ```

    </div>

    </div>

    **Total: 4pts**  

5.  **changePassword (1 API)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="36a0eafd-6f67-4576-b3b5-12af5b39a36e" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/').post(controller.postChangePassword); 5pts
    ```

    </div>

    </div>

    **Total: 5pts**

6.  **clientLog (1 API)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="b4a5d5e6-f116-4b87-8960-5bcff473facf" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/').post(controller.postClientLog); 6pts (log also in DB)
    ```

    </div>

    </div>

    **Total: 6pts**  

7.  **cobDossierApiProxy (1 API)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0687510e-315b-4d95-9b71-14f16c29d375" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/:tenant/:cobId').get(controller.getDossierJson); 0pts (external API /ivy/api/Portal/v1/dossiers/ may return stream)
    ```

    </div>

    </div>

    **Total: 0pts**  

8.  **cobIdScanApiResolver (3 APIs)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="9cf58b01-480a-4860-bc40-81e5d830d7b2" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/image/:id').get(controller.getImageBinary); 3pts
    router.route('/orig/:tenant/:cobId').get(controller.getOrigBinary); 3pts
    router.route('/:tenant/:cobId').get(controller.getMetaArray); 5pts
    ```

    </div>

    </div>

    **Total: 11pts**  

9.  **coComments (5 APIs)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="a7672bc0-a9b2-4043-9160-b9a94add0fd2" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/add').post(controller.postCoComment); 8pts
    router.route('/update').put(controller.putCoComment); 8pts
    router.route('/getByDossierId').post(controller.getCoCommentByDossierId); 5pts
    router.route('/list').get(controller.listCoComments); 0pts (not exist logic of query)
    router.route('/delete').post(controller.deleteCoComment); 3pts
    ```

    </div>

    </div>

    **Total: 24pts**  

10. **config (6 APIs)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="39ee5a45-4a32-43ba-a36e-24cc8669e35a" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/serviceTime').put(controller.putServiceTime); 10pts
    router.route('/serviceTime').get(controller.getServiceTime); 6pts
    router.route('/holiday').post(controller.postHoliday); 6pts
    router.route('/holiday').put(controller.putHoliday); 5pts
    router.route('/holiday/list').get(controller.listHolidays); 6pts
    router.route('/holiday/delete').post(controller.deleteHoliday); 3pts
    ```

    </div>

    </div>

    **Total: 36pts**  

11. **dwhResolver (1 API)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="072dc234-11d4-4cae-88e1-a57a1657da77" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/search').post(controller.search); 0pts (the involved tables may already exist, may ask Christian for them)
    ```

    </div>

    </div>

    **Total: 0pts**  

12. **export (2 APIs)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="3ad60d0c-b5bf-4790-933c-1c1dcaec716f" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/coComments').get(controller.getCoCommentExport);8pts
    router.route('/activeSessions').get(controller.getActiveSessions); 0pts (doesn't use DB, consider skip because not used by FE and the API response is ExpressJS specific)
    ```

    </div>

    </div>

    **Total: 8pts**  

13. **healthcheck (1 API)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="5eee1c69-c91b-4ef2-ae39-4566b72b855b" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/sobHealthCheckResult').get(controller.getSobHealthCheckResult); 0pts (doesn't use DB)
    ```

    </div>

    </div>

    **Total: 0pts**  

14. **idnowApiProxy (2 APIs)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="aa526449-72ff-4a0d-8eca-9d905b445464" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/list').get(controller.getList); 0pts (doesn't use DB, may contact Christian)
    router.route('/zip/:transactionId').get(controller.getZip); 0pts (doesn't use DB, may contact Christian)
    ```

    </div>

    </div>

    **Total: 0pts**  

15. **kyccheck (5 APIs)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="873a4626-b3ee-4a4f-9600-97e04e265f6e" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/person/search').post(controller.postSearchPerson); 0pts (doesn't use DB)
    router.route('/person/personProfileDetail').post(controller.postPersonProfileDetail); 0pts (doesn't use DB)
    router.route('/person/indexStatistics').get(controller.getIndexStatistics); 0pts (doesn't use DB)
    router.route('/person/indexVersions').get(controller.getIndexVersions); 0pts (doesn't use DB)
    router.route('/person/personsToCheck').post(controller.getPersonsToCheck); 0pts (doesn't use DB)
    ```

    </div>

    </div>

    **Total: 0pts**  

16. **links (4 APIs)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="56c8cc10-3956-49ab-9994-29346291cff5" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/deleteLink').post(controller.deleteLink); 2pts
    router.route('/').post(controller.postLink); 3pts
    router.route('/').put(controller.putLink); 3pts
    router.route('/').get(controller.getLinks); 3pts
    ```

    </div>

    </div>

    **Total: 11pts**  

17. **livereporting (1 API)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="85970381-bbab-4ab3-ace2-e39859c28141" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/getBasicReport').post(controller.getBasicReport); 0pts (doesn't use DB)
    ```

    </div>

    </div>

    **Total: 0pts**

18. **logout (3 APIs)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="1616f07e-53c4-4f24-ac27-8fb91d16f073" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/checkOut').post(controller.postLogoutUser); 5pts
    router.route('/checkIn').post(controller.postLogInUser); 5pts
    router.route('/').post(controller.postLogout); 6pts
    ```

    </div>

    </div>

    **Total: 16pts**  

19. **messages (2 APIs)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0e07f1cd-4144-4e49-a120-451af0fb9630" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/').put(controller.putMessage); 4pts
    router.route('/').get(controller.getMessage); 4pts
    ```

    </div>

    </div>

    **Total: 8pts**  

20. **operationalplan (6 APIs)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="0e31c530-7b62-4a10-bdfa-8fce54285279" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/').post(controller.postOperationalPlan); 5pts
    router.route('/').get(controller.listOperationalPlans); 6pts
    router.route('/delete').put(controller.putDeleteOperationalPlan); 3pts
    router.route('/updateGrp/').put(controller.putUpdateGrp); 6pts
    router.route('/updateSv/').put(controller.putUpdateSv); 6pts
    router.route('/reset/').get(controller.getResetOperationalPlans); 4pts
    ```

    </div>

    </div>

    **Total: 30pts**  

21. **route (14 APIs)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="e610fb48-31aa-49f6-a033-155eb9b296da" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/solveCallRecoms').get(controller.solveCallRecoms); 15pts
    router.route('/solveCobTasks').get(controller.solveCobTasks); 10pts
    router.route('/solveCobChats').get(controller.solveCobChats); 10pts
    router.route('/deleteOldAlerts').get(controller.deleteOldAlerts); 3pts
    router.route('/deleteOldChatAlerts').get(controller.deleteOldChatAlerts); 3pts
    router.route('/deleteOldCallRecoms').get(controller.deleteOldCallRecoms); 3pts
    router.route('/logOutAllUsers').get(controller.logOutAllUsers); 4pts
    router.route('/deleteOldSupervisorRequests').get(controller.deleteOldSupervisorRequest); 4pts
    router.route('/deleteAllAlertNotifications').get(controller.deleteAllAlertNotifications); 3pts
    router.route('/deleteAllChatAlertNotifications').get(controller.deleteAllChatAlertNotifications); 3pts
    router.route('/deleteAllSupervisorNotifications').get(controller.deleteAllSupervisorNotifications); 3pts
    router.route('/deleteOldCoComments').get(controller.deleteOldCoComments); 3pts
    router.route('/loadOperationalPlans').get(controller.loadOperationalPlans); 10pts
    router.route('/deleteAllCobIdScanData').get(controller.deleteAllCobIdScanData); 3pts
    ```

    </div>

    </div>

    **Total: 77pts**

22. **supervisor (7 APIs)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="80c94d46-dbad-4e97-be20-f74afe784e2a" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/deleteSupervisorRequest').post(controller.deleteSupervisorRequest); 2pts
    router.route('/').post(controller.postSupervisorRequest); 6pts
    router.route('/').put(controller.putSupervisorRequest); 6pts
    router.route('/allSupervisorRequests').get(controller.getSupervisorRequests); 5pts
    router.route('/openSupervisorRequests').get(controller.getOpenSupervisorRequests); 5pts 
    router.route('/addSupervisorNotification').post(controller.addSupervisorNotification); 4pts 
    router.route('/').get(controller.getSupervisorRequest); 4pts
    ```

    </div>

    </div>

    **Total: 32pts**  

23. **tasks (4 APIs)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="db0befb5-c2ee-4e5a-aeb2-c97b8108b99c" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/deleteTaskInfo').post(controller.deleteTaskInfo); 4pts
    router.route('/').post(controller.postTaskInfo); 5pts
    router.route('/').put(controller.putTaskInfo); 5pts
    router.route('/').get(controller.getTaskInfos); 6pts
    ```

    </div>

    </div>

    **Total: 20pts**  

24. **userActions (1 API)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="c0dca205-456f-427b-b488-e3bf1a636ba9" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/').post(controller.postUserAction); 4pts
    ```

    </div>

    </div>

    **Total: 4pts**

25. **users (10 APIs)**

    <div class="code panel pdl conf-macro output-block" hasbody="true" macro-id="eaabcc89-3764-4811-8583-692ac8f95be4" macro-name="code" style="border-width: 1px;">

    <div class="codeContent panelContent pdl">

    ``` syntaxhighlighter-pre
    router.route('/getUserData').post(controller.getUserData); 6pts (may join with other tables e.g. priorities)
    router.route('/deleteUser').post(controller.deleteUser); 2pts
    router.route('/updateAgentGroup').put(controller.UpdateAgentGroup); 4pts
    router.route('/updateIsSupervisorActiv').put(controller.UpdateIsSupervisorActiv); 3pts
    router.route('/updateIsSupervisorAvailable').put(controller.UpdateIsSupervisorAvailable); 3pts

    router.route('/statActiveAgentsPerAgentGroup').get(controller.getStatisticsActiveAgentsPerAgentGroup); 5pts
    router.route('/statActiveAgentsPerLang').get(controller.getStatisticsActiveAgentsPerLang); 5pts

    router.route('/').post(controller.postUser); 5pts
    router.route('/').get(controller.getUsers); 5pts
    router.route('/').put(controller.putUser); 5pts
    ```

    </div>

    </div>

    **Total: 43pts**

##  4. Split requirements

- Research (NodeJS + Postgres stack) + integration + implement commons: ORM, Migration tool **15pts**

- Create the Postgres ORM Entity + migration changesets for existing MongoDB models using the Migration tool: **30pts** (<a href="#Data-models" rel="nofollow">Data models</a>)

- Implement the migration of the existing data to the new Database **10pts + 30pts = 40pts**

  - Research (may need confirmation with AT) about data migration **10pts**

  - Apply the solution for all the tables **30pts**

- Change or keep the current authentication mechanism?

- Adapt the current APIs **335pts**

  - Implement the APIs + tests again (<a href="#APIs-that-needs-to-adapt" rel="nofollow">APIs that need to adapt</a>) **335pts**

**TOTAL: 420pts**

## 5. Questions

1.  Which approach should we choose: Quarkus-based (**1135pts**) or NodeJS-based (**420pts**)?

2.  Which should we choose for Database structure migration (like Liquibase)? <a href="#1.1.-Database-migration" rel="nofollow">1.1. Database migration</a>

3.  Which should we choose for Database ORM (like Hibernate)? <a href="#1.2.-Database-ORM-Framework" rel="nofollow">1.2. Database ORM Framework</a>

4.  We need to “have an automatic mechanism for the one-time data migration”: at the moment we don’t have any options yet, could AT suggest us an approach?

5.  If we choose the approach of NodeJS-based, can we cancel the ticket <span class="confluence-jim-macro jira-issue conf-macro output-block" client-id="SINGLE_d3f195c5-8684-3b17-b4f6-e9ee3a0b0fe2_48139206657_712020:87b0f7f1-aaab-4406-a25d-fa0fc075c4d4" hasbody="false" jira-key="FA-1384" macro-id="972647ca-8d72-4b52-a19c-0d5074d5797b" macro-name="jira"> <a href="https://axonivy.atlassian.net/browse/FA-1384" class="jira-issue-key"><span class="aui-icon aui-icon-wait issue-placeholder"> </span>FA-1384</a> - <span class="summary">Getting issue details...</span> <span class="aui-lozenge aui-lozenge-subtle aui-lozenge-default issue-placeholder">STATUS</span> </span> ?

6.  If we continuously go on with the Authentication with Keycloak and AXIN services for NodeJS, is there any document or tutorial to set up and implement it?

7.  If we don't continue with KeyCloak and AXIN services for NodeJS, should we keep the current Session-based authentication or use another solution such as JWT in NodeJS?
