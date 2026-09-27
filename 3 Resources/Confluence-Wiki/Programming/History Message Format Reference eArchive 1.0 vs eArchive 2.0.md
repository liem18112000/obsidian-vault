---
title: "History Message Format Reference: eArchive 1.0 vs eArchive 2.0"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/Helios/pages/49447829524/History+Message+Format+Reference+eArchive+1.0+vs+eArchive+2.0
space: "Helios"
topic: programming
relevance: 0.777
depth: 2.92
updated: 2026-06-18
attachments: 2
tags:
  - confluence
  - programming
  - space/helios
---

# History Message Format Reference: eArchive 1.0 vs eArchive 2.0

> [!info] Imported from Confluence
> Space **Helios** · updated 2026-06-18 · [open original](https://axonivy.atlassian.net/wiki/spaces/Helios/pages/49447829524/History+Message+Format+Reference+eArchive+1.0+vs+eArchive+2.0)
> Relevance 0.777 · topic `programming`

# eArchive 1.0 — Letter Detail History Analysis

**Sources analysed:**

- API model: `luz_docs_view_controller` — all classes extending `ActionAuditEntry`

- Web layer: `luz_epost_business_web` — `LetterHistoryExtractor`, `LetterHistoryType`, all `*History.java` classes, `cms_en.yaml`

- Entry point: `LetterDetailHandler.buildLetterHistories(letter)` → `LetterHistoryExtractor.extractFrom(letter)`

------------------------------------------------------------------------

## How the history list is built

`LetterHistoryExtractor.extractFrom(LetterWebModel)` produces a flat, unsorted list of `LetterHistory` objects by:

1.  **Standard histories** – built from scalar date fields on the Letter (no actor): `ORIGINAL_REQUESTED`, `TRANSFERRED_TO_BOOKING`, `USER_SIGNED`. These are created inside `buildStandardLetterHistories()`. `RECEIVED` also has `isStandardHistory = true` (renders without actor) but is **not** built in that method — it is created in the if/else block directly inside `extractFrom()` and added to `letterHistories`.

2.  **Storage histories** – built from `letter.storageHistoryEntries` via `LetterStorageHistoryFactory` (action field → `LetterHistoryType`).

3.  **Per-entry lists** – each element of a list property maps to one history row.

4.  **Tag / PlainText / DocumentType / SecurityClass modification factories** – delegate to their own factory per entry.

Rendered-check: every `AuditActionHistory` has a `rendered` boolean. It is set to `true` only when `dateTime != null`. Items with `rendered = false` are hidden by the template.

------------------------------------------------------------------------

## History Table

<div>

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr>
<th></th>
<th><p>History Event</p></th>
<th><p>Letter Property (<code>Letter.java</code>)</p></th>
<th><p>Render Rule</p></th>
<th><p>English Display Message</p></th>
</tr>
&#10;<tr>
<td>1</td>
<td><p>Received</p></td>
<td><p><code>createDate: String</code></p></td>
<td><p><code>receivedDate != null</code> AND <code>letterOrigin</code> is not <code>ELETTER</code>, <code>SCAN_CENTERS</code>, or uploaded</p></td>
<td><p>"Received"</p></td>
</tr>
<tr>
<td>2</td>
<td><p>Received via ePost</p></td>
<td><p><code>createDate: String</code></p></td>
<td><p><code>receivedDate != null</code> AND <code>letterOrigin == ELETTER</code></p></td>
<td><p>"eLetter received via ePost"</p></td>
</tr>
<tr>
<td>3</td>
<td><p>Received from scan centre</p></td>
<td><p><code>createDate: String</code></p></td>
<td><p><code>receivedDate != null</code> AND <code>letterOrigin == SCAN_CENTERS</code></p></td>
<td><p>"Document received from scan centre"</p></td>
</tr>
<tr>
<td>4</td>
<td><p>Uploaded</p></td>
<td><p><code>uploadedHistoryEntry: UploadedHistoryEntry</code></p></td>
<td><p><code>letterOrigin.isUploaded() == true</code> AND <code>uploadedHistoryEntry != null</code></p></td>
<td><p>"{actor} uploaded this document" + folder name</p></td>
</tr>
<tr>
<td>5</td>
<td><p>Read</p></td>
<td><p><code>readHistoryEntries: List&lt;ReadHistoryEntry&gt;</code></p></td>
<td><p>One row per entry where <code>atTime != null</code></p></td>
<td><p>"{actor} read this document"</p></td>
</tr>
<tr>
<td>6</td>
<td><p>Downloaded</p></td>
<td><p><code>downloadHistoryEntries: List&lt;DownloadHistoryEntry&gt;</code></p></td>
<td><p>One row per entry</p></td>
<td><p>"{actor} downloaded this document {timestamp}"</p></td>
</tr>
<tr>
<td>7</td>
<td><p>Printed</p></td>
<td><p><code>printHistoryEntries: List&lt;PrintHistoryEntry&gt;</code></p></td>
<td><p>One row per entry</p></td>
<td><p>"{actor} printed this document {timestamp}"</p></td>
</tr>
<tr>
<td>8</td>
<td><p>Exported (zip)</p></td>
<td><p><code>exportHistoryEntries: List&lt;ExportHistoryEntry&gt;</code></p></td>
<td><p>One row per entry</p></td>
<td><p>"{actor} exported this document {timestamp}."</p></td>
</tr>
<tr>
<td>9</td>
<td><p>Stored to folder</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == STORE</code> AND <code>toStorageLocation != ROOT</code></p></td>
<td><p>"{actor} stored this document to {folder}" <em>(eArchive: "archived … to")</em></p></td>
</tr>
<tr>
<td>10</td>
<td><p>Stored outside folders</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == STORE</code> AND <code>toStorageLocation == ROOT</code></p></td>
<td><p>"{actor} archived this document outside folders"</p></td>
</tr>
<tr>
<td>11</td>
<td><p>Copied to folder</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == COPY</code></p></td>
<td><p>"{actor} copied this document to {folder}"</p></td>
</tr>
<tr>
<td>12</td>
<td><p>Moved (folder → folder)</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == MOVE</code> AND <code>fromStorageLocation != ROOT</code> AND <code>toStorageLocation != ROOT</code></p></td>
<td><p>"{actor} added this document to {to}" + "and removed it from {from}"</p></td>
</tr>
<tr>
<td>13</td>
<td><p>Moved to root (removed from all folders)</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == MOVE</code> AND <code>toStorageLocation == ROOT</code></p></td>
<td><p>"{actor} removed this file from all folders"</p></td>
</tr>
<tr>
<td>14</td>
<td><p>Moved from root (added to folder)</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == MOVE</code> AND <code>fromStorageLocation == ROOT</code></p></td>
<td><p>"{actor} added this document to {folder}"</p></td>
</tr>
<tr>
<td>15</td>
<td><p>Removed from folder</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == REMOVE</code></p></td>
<td><p>"{actor} removed this document from {folder}"</p></td>
</tr>
<tr>
<td>16</td>
<td><p>Undo store (folder)</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == UNDO_STORING</code> AND <code>fromStorageLocation != ROOT</code></p></td>
<td><p>"{actor} has undone storing in {folder}" <em>(eArchive: "undone archive in")</em></p></td>
</tr>
<tr>
<td>17</td>
<td><p>Undo store (root)</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == UNDO_STORING</code> AND <code>fromStorageLocation == ROOT</code></p></td>
<td><p>"{actor} undone archive outside folders"</p></td>
</tr>
<tr>
<td>18</td>
<td><p>Undo copy</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == UNDO_COPYING</code></p></td>
<td><p>"{actor} has undone copying to {folder}"</p></td>
</tr>
<tr>
<td>19</td>
<td><p>Undo move (folder → folder)</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == UNDO_MOVING</code> AND <code>fromStorageLocation != ROOT</code> AND <code>toStorageLocation != ROOT</code></p></td>
<td><p>"{actor} has undone the adding of this document to {to}" + "and the removal from {from}"</p></td>
</tr>
<tr>
<td>20</td>
<td><p>Undo move from root</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == UNDO_MOVING</code> AND <code>fromStorageLocation == ROOT</code></p></td>
<td><p>"{actor} has undone the removal of all folders"</p></td>
</tr>
<tr>
<td>21</td>
<td><p>Undo move to root</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == UNDO_MOVING</code> AND <code>toStorageLocation == ROOT</code></p></td>
<td><p>"{actor} has undone removal from {folder}"</p></td>
</tr>
<tr>
<td>22</td>
<td><p>Undo remove</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == UNDO_REMOVING</code></p></td>
<td><p>"{actor} has undone removal from folder {folder}"</p></td>
</tr>
<tr>
<td>23</td>
<td><p>Restored from trash (to folder)</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == RESTORE</code> AND <code>toStorageLocation == FOLDER</code> AND <code>destinationDirectoryIds</code> is not empty</p></td>
<td><p>"{actor} restored this document from trash to {folder}"</p></td>
</tr>
<tr>
<td>24</td>
<td><p>Restored from trash (to inbox)</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == RESTORE</code> AND <code>toStorageLocation == INBOX</code></p></td>
<td><p>"{actor} restored this document from trash to the inbox"</p></td>
</tr>
<tr>
<td>25</td>
<td><p>Restored from trash (to eArchive root)</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == RESTORE</code> AND <code>toStorageLocation == ROOT</code></p></td>
<td><p>"{actor} restored this document from trash to eArchive"</p></td>
</tr>
<tr>
<td>26</td>
<td><p>Restored passively (folder restore triggered)</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == RESTORE</code> AND <code>toStorageLocation == FOLDER</code> AND <code>destinationDirectoryIds</code> is empty</p></td>
<td><p>"{actor} restored this document from Trash"</p></td>
</tr>
<tr>
<td>27</td>
<td><p>Undo restore (to folder)</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == UNDO_RESTORING</code> AND <code>fromStorageLocation</code> is folder</p></td>
<td><p>"{actor} undid restoration to {folder}"</p></td>
</tr>
<tr>
<td>28</td>
<td><p>Undo restore (to inbox)</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == UNDO_RESTORING</code> AND <code>fromStorageLocation == INBOX</code></p></td>
<td><p>"{actor} undid restoration to the inbox"</p></td>
</tr>
<tr>
<td>29</td>
<td><p>Undo restore (to eArchive root)</p></td>
<td><p><code>storageHistoryEntries: List&lt;LetterStorageHistoryEntry&gt;</code></p></td>
<td><p><code>action == UNDO_RESTORING</code> AND <code>fromStorageLocation == ROOT</code></p></td>
<td><p>"{actor} undid restoration to eArchive"</p></td>
</tr>
<tr>
<td>30</td>
<td><p>Deleted by user</p></td>
<td><p><code>deletingHistoryEntries: List&lt;DeletingHistoryEntry&gt;</code></p></td>
<td><p>One row per entry where <code>deletionCause != END_OF_GRACE_PERIOD</code></p></td>
<td><p>"{actor} deleted this document {timestamp}"</p></td>
</tr>
<tr>
<td>31</td>
<td><p>Deleted by system (subscription ended)</p></td>
<td><p><code>deletingHistoryEntries: List&lt;DeletingHistoryEntry&gt;</code></p></td>
<td><p>One row per entry where <code>deletionCause == END_OF_GRACE_PERIOD</code></p></td>
<td><p>"This document was deleted by system, due to end of eArchive subscription {timestamp}."</p></td>
</tr>
<tr>
<td>32</td>
<td><p>Undo deletion</p></td>
<td><p><code>undoDeletingHistoryEntries: List&lt;UndoDeletingHistoryEntry&gt;</code></p></td>
<td><p>One row per entry</p></td>
<td><p>"{actor} has undone deletion {timestamp}"</p></td>
</tr>
<tr>
<td>33</td>
<td><p>Physical letter requested (date only)</p></td>
<td><p><code>physicalOrderedDate: LocalDate</code></p></td>
<td><p><code>physicalOrderedDate != null</code></p></td>
<td><p>"Physical letter requested on {relative date}"</p></td>
</tr>
<tr>
<td>34</td>
<td><p>User requested physical original</p></td>
<td><p><code>requestOriginalHistoryEntry: RequestOriginalHistoryEntry</code></p></td>
<td><p><code>requestOriginalHistoryEntry != null</code> AND <code>atTime != null</code></p></td>
<td><p>"{actor} ordered the physical original to the following [address] {date} at {time}."</p></td>
</tr>
<tr>
<td>35</td>
<td><p>Transferred to booking (date only)</p></td>
<td><p><code>transferredToBookingDate: LocalDate</code></p></td>
<td><p><code>transferredToBookingDate != null</code></p></td>
<td><p>"Transferred to booking on {relative date}"</p></td>
</tr>
<tr>
<td>36</td>
<td><p>User transferred to booking</p></td>
<td><p><code>transferredToBookingHistoryEntry: TransferredToBookingHistoryEntry</code></p></td>
<td><p><code>transferredToBookingHistoryEntry != null</code></p></td>
<td><p>"{actor} transferred this document to accounting {timestamp}"</p></td>
</tr>
<tr>
<td>37</td>
<td><p>Tag added</p></td>
<td><p><code>tagModificationHistoryEntries: List&lt;TagModificationHistoryEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == "add_element"</code></p></td>
<td><p>"{actor} added the tag {tag value}"</p></td>
</tr>
<tr>
<td>38</td>
<td><p>Tag removed</p></td>
<td><p><code>tagModificationHistoryEntries: List&lt;TagModificationHistoryEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == "remove_element"</code></p></td>
<td><p>"{actor} removed the tag {tag value}"</p></td>
</tr>
<tr>
<td>39</td>
<td><p>Title added</p></td>
<td><p><code>letterPlainTextPropertyModificationEntries: List&lt;LetterPlainTextPropertyModificationEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == ADD_FIELD</code> AND <code>field == "title"</code></p></td>
<td><p>"{actor} added this [document title] {timestamp}"</p></td>
</tr>
<tr>
<td>40</td>
<td><p>Title changed</p></td>
<td><p><code>letterPlainTextPropertyModificationEntries: List&lt;LetterPlainTextPropertyModificationEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == REPLACE_FIELD</code> AND <code>field == "title"</code></p></td>
<td><p>"{actor} changed the document title from [old] to [new] {timestamp}"</p></td>
</tr>
<tr>
<td>41</td>
<td><p>Description added</p></td>
<td><p><code>letterPlainTextPropertyModificationEntries: List&lt;LetterPlainTextPropertyModificationEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == ADD_FIELD</code> AND <code>field == "description"</code></p></td>
<td><p>"{actor} added this [document description] {timestamp}"</p></td>
</tr>
<tr>
<td>42</td>
<td><p>Description changed</p></td>
<td><p><code>letterPlainTextPropertyModificationEntries: List&lt;LetterPlainTextPropertyModificationEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == REPLACE_FIELD</code> AND <code>field == "description"</code></p></td>
<td><p>"{actor} changed the document description from [old] to [new] {timestamp}"</p></td>
</tr>
<tr>
<td>43</td>
<td><p>Description removed</p></td>
<td><p><code>letterPlainTextPropertyModificationEntries: List&lt;LetterPlainTextPropertyModificationEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == REMOVE_FIELD</code> AND <code>field == "description"</code></p></td>
<td><p>"{actor} removed the document description {timestamp}"</p></td>
</tr>
<tr>
<td>44</td>
<td><p>Invoice amount added</p></td>
<td><p><code>letterPlainTextPropertyModificationEntries: List&lt;LetterPlainTextPropertyModificationEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == ADD_FIELD</code> AND <code>field == "invoiceAmount"</code></p></td>
<td><p>"{actor} added the amount {amount} {timestamp}"</p></td>
</tr>
<tr>
<td>45</td>
<td><p>Invoice amount changed</p></td>
<td><p><code>letterPlainTextPropertyModificationEntries: List&lt;LetterPlainTextPropertyModificationEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == REPLACE_FIELD</code> AND <code>field == "invoiceAmount"</code></p></td>
<td><p>"{actor} changed the amount from {old} to {new} {timestamp}"</p></td>
</tr>
<tr>
<td>46</td>
<td><p>Invoice amount removed</p></td>
<td><p><code>letterPlainTextPropertyModificationEntries: List&lt;LetterPlainTextPropertyModificationEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == REMOVE_FIELD</code> AND <code>field == "invoiceAmount"</code></p></td>
<td><p>"{actor} removed the amount {timestamp}."</p></td>
</tr>
<tr>
<td>47</td>
<td><p>Invoice due date added</p></td>
<td><p><code>letterPlainTextPropertyModificationEntries: List&lt;LetterPlainTextPropertyModificationEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == ADD_FIELD</code> AND <code>field == "invoiceDueDate"</code></p></td>
<td><p>"{actor} added the due date {date} {timestamp}"</p></td>
</tr>
<tr>
<td>48</td>
<td><p>Invoice due date changed</p></td>
<td><p><code>letterPlainTextPropertyModificationEntries: List&lt;LetterPlainTextPropertyModificationEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == REPLACE_FIELD</code> AND <code>field == "invoiceDueDate"</code></p></td>
<td><p>"{actor} changed the due date from {old} to {new} {timestamp}"</p></td>
</tr>
<tr>
<td>49</td>
<td><p>Invoice due date removed</p></td>
<td><p><code>letterPlainTextPropertyModificationEntries: List&lt;LetterPlainTextPropertyModificationEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == REMOVE_FIELD</code> AND <code>field == "invoiceDueDate"</code></p></td>
<td><p>"{actor} removed the due date {timestamp}."</p></td>
</tr>
<tr>
<td>50</td>
<td><p>Reference date added</p></td>
<td><p><code>letterPlainTextPropertyModificationEntries: List&lt;LetterPlainTextPropertyModificationEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == ADD_FIELD</code> AND <code>field == "referenceDate"</code></p></td>
<td><p>"{actor} added this document date {date} {timestamp}"</p></td>
</tr>
<tr>
<td>51</td>
<td><p>Reference date changed</p></td>
<td><p><code>letterPlainTextPropertyModificationEntries: List&lt;LetterPlainTextPropertyModificationEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == REPLACE_FIELD</code> AND <code>field == "referenceDate"</code></p></td>
<td><p>"{actor} changed the document date from {old} to {new} {timestamp}"</p></td>
</tr>
<tr>
<td>52</td>
<td><p>Reference date removed</p></td>
<td><p><code>letterPlainTextPropertyModificationEntries: List&lt;LetterPlainTextPropertyModificationEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == REMOVE_FIELD</code> AND <code>field == "referenceDate"</code></p></td>
<td><p>"{actor} removed the document date {timestamp}"</p></td>
</tr>
<tr>
<td>53</td>
<td><p>Document type added</p></td>
<td><p><code>documentTypeModificationHistoryEntries: List&lt;DocumentTypeModificationHistoryEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == ADD_FIELD</code></p></td>
<td><p>"{actor} assigned this [document type] {timestamp}."</p></td>
</tr>
<tr>
<td>54</td>
<td><p>Document type replaced</p></td>
<td><p><code>documentTypeModificationHistoryEntries: List&lt;DocumentTypeModificationHistoryEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == REPLACE_FIELD</code></p></td>
<td><p>"{actor} changed the document type from {old} to {new} {timestamp}."</p></td>
</tr>
<tr>
<td>55</td>
<td><p>Document type removed</p></td>
<td><p><code>documentTypeModificationHistoryEntries: List&lt;DocumentTypeModificationHistoryEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == REMOVE_FIELD</code></p></td>
<td><p>"{actor} removed the document type {timestamp}."</p></td>
</tr>
<tr>
<td>56</td>
<td><p>Security class added</p></td>
<td><p><code>securityClassModificationHistoryEntries: List&lt;SecurityClassModificationHistoryEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == "ADD_ELEMENT"</code></p></td>
<td><p>"{actor} added the security class "{name}""</p></td>
</tr>
<tr>
<td>57</td>
<td><p>Security class removed</p></td>
<td><p><code>securityClassModificationHistoryEntries: List&lt;SecurityClassModificationHistoryEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == "REMOVE_ELEMENT"</code></p></td>
<td><p>"{actor} removed the security class "{name}""</p></td>
</tr>
<tr>
<td>58</td>
<td><p>All security classes disabled (subscription downgrade)</p></td>
<td><p><code>securityClassModificationHistoryEntries: List&lt;SecurityClassModificationHistoryEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == "DISABLED_ALL_ELEMENTS"</code></p></td>
<td><p>"Assigned access classes have been disabled because {actor} unsubscribed from eArchive Plus {timestamp}"</p></td>
</tr>
<tr>
<td>59</td>
<td><p>All security classes re-enabled (re-subscribed)</p></td>
<td><p><code>securityClassModificationHistoryEntries: List&lt;SecurityClassModificationHistoryEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == "ENABLED_ALL_ELEMENTS"</code></p></td>
<td><p>"Assigned access classes have been enabled again because {actor} has resubscribed eArchive Plus {timestamp}"</p></td>
</tr>
<tr>
<td>60</td>
<td><p>All security classes removed (re-subscribed)</p></td>
<td><p><code>securityClassModificationHistoryEntries: List&lt;SecurityClassModificationHistoryEntry&gt;</code></p></td>
<td><p>One row per entry where <code>action == "REMOVE_ALL_ELEMENTS"</code></p></td>
<td><p>"{actor} has re-subscribed eArchive Plus {timestamp} and removed all assigned access classes."</p></td>
</tr>
<tr>
<td>61</td>
<td><p>SES digitally signed</p></td>
<td><p><code>documentSignature: DocumentSignature</code><br />
<code>SignatureInfo { signingDateTime: LocalDateTime, user: UserInfo }</code></p></td>
<td><p><code>documentSignature.signatures[0].signingDateTime != null</code></p></td>
<td><p>"Document was digitally signed by {actor} {timestamp}."</p></td>
</tr>
</tbody>
</table>

</div>

------------------------------------------------------------------------

## Notes on display rendering

<div>

|  |  |  |
|----|----|----|
|  | Aspect | Detail |
| 1 | **Timestamp format** | Date+time histories: `"{date} at {time}"`. Date-only (standard) histories: `"on {relative date}"`. |
| 2 | **Actor name** | Resolved from `byUser` (username) via `IUserRepository.findWithExternalLookup()`; falls back to raw username if not found. |
| 3 | **Folder names** | Resolved from `relatedFolders` map (`Map<folderId, LetterStorageFolderWebModel>`); if not found, shown as `UNDEFINED`. |
| 4 | **Multi-folder display** | For folder-based events (stored, copied, moved, removed, restored, undo variants), the template renders differently by count: **1 folder** → clickable `p:commandLink`; **≥ 2 folders** → a count badge (`"N folders"`) with a hover tooltip listing all folder links. |
| 5 | `isStandardHistory` | `RECEIVED`, `ORIGINAL_REQUESTED`, `TRANSFERRED_TO_BOOKING`, `USER_SIGNED` have this flag = `true` in `LetterHistoryType`. No actor is shown for those using the date-only variant. |
| 6 | `USER_SIGNED` **visibility** | `LetterSesSignedHistory` is **always instantiated** in `buildStandardLetterHistories()`, but `rendered` is set to `false` when `documentSignature.signatures[0].signingDateTime` is `null` (i.e. the letter has no SES digital signature). Only SES-signed letters will show this history entry. For all other letters (regular ePost, uploaded, scan-centre), it is silently suppressed. Source: `AuditActionHistory.setHistoryInfo(…, LocalDateTime)` — `rendered = Objects.nonNull(atDateTime)`. |
| 7 | **Why "Signed / System" may be missing** | The history row does **not** use top-level `signedDate`; it uses only `documentSignature.signatures[0].signingDateTime`. If payload has `signedDate` but no SES `documentSignature` (or empty signatures), history `USER_SIGNED` is not rendered. Also, actor text comes from SES `signatureInfo.user` (name/email), not a fixed `System` label. |
| 8 | **eArchive feature flag** | `FeatureSwitchBean.isEArchiveEnabled()` changes label for `STORED` ("archived") and `UNDONE_STORING` ("undone archive"). |
| 9 | **Unread history** | `unreadHistoryEntries` exists in the API model (`Letter.unreadHistoryEntries`) but is **not rendered** in the detail history list. |
| 10 | **Invoice transmitted** | `invoiceTransmittedHistoryEntries` / `invoiceUntransmittedHistoryEntries` exist in the API model but are **not rendered** in the detail history list. |
| 11 | **Registered letter accepted** | `registeredLetterAcceptedHistoryEntries` exists in the API model but is **not rendered** in the detail history list. |
| 12 | **Sort order** | Not sorted; the list is returned in insertion order by `LetterHistoryExtractor`. |

</div>

------------------------------------------------------------------------

## API model → `Letter` property mapping (luz_docs_view_controller)

<div>

|  |  |  |  |
|----|----|----|----|
|  | Letter property | API class / type | Source |
| 1 | `createDate` | `String` | Becomes `receivedDate` in web entity via `LetterResponse.getLetter()` |
| 2 | `uploadedHistoryEntry` | `UploadedHistoryEntry` | Fields: `byUser`, `atTime`, `folderId` |
| 3 | `readHistoryEntries` | `List<ReadHistoryEntry>` | Fields: `byUser`, `atTime` |
| 4 | `unreadHistoryEntries` | `List<UnreadHistoryEntry>` | Not displayed |
| 5 | `downloadHistoryEntries` | `List<DownloadHistoryEntry>` | Fields: `byUser`, `atTime` |
| 6 | `printHistoryEntries` | `List<PrintHistoryEntry>` | Fields: `byUser`, `atTime` |
| 7 | `exportHistoryEntries` | `List<ExportHistoryEntry>` | Fields: `byUser`, `atTime` |
| 8 | `storageHistoryEntries` | `List<LetterStorageHistoryEntry>` | Fields: `byUser`, `atTime`, `action` (StorageAction), `sourceDirectoryIds`, `destinationDirectoryIds`, `fromStorageLocation`, `toStorageLocation` |
| 9 | `deletingHistoryEntries` | `List<DeletingHistoryEntry>` | Fields: `byUser`, `atTime`, `deletionCause`, `fromDirectoryIds` |
| 10 | `undoDeletingHistoryEntries` | `List<UndoDeletingHistoryEntry>` | Fields: `byUser`, `atTime` |
| 11 | `physicalOrderedDate` | `LocalDate` | Date-only, no actor |
| 12 | `requestOriginalHistoryEntry` | `RequestOriginalHistoryEntry` | Fields: `byUser`, `atTime`, `deliveryAddress` |
| 13 | `transferredToBookingDate` | `LocalDate` | Date-only, no actor |
| 14 | `transferredToBookingHistoryEntry` | `TransferredToBookingHistoryEntry` | Fields: `byUser`, `atTime` |
| 15 | `tagModificationHistoryEntries` | `List<TagModificationHistoryEntry>` | Fields: `byUser`, `atTime`, `tag`, `action` ("add_element" / "remove_element") |
| 16 | `letterPlainTextPropertyModificationEntries` | `List<LetterPlainTextPropertyModificationEntry>` | Fields: `byUser`, `atTime`, `action` (LetterPropertyModificationAction enum), `field` (fieldName string), `oldValue`, `newValue` |
| 17 | `documentTypeModificationHistoryEntries` | `List<DocumentTypeModificationHistoryEntry>` | Fields: `byUser`, `atTime`, `action` (LetterPropertyModificationAction), `oldType`, `newType` |
| 18 | `securityClassModificationHistoryEntries` | `List<SecurityClassModificationHistoryEntry>` | Fields: `byUser`, `atTime`, `action` (String), `securityClassCode`, `securityClassName` |
| 19 | `signedDate` | `Instant` | Used for signature stamp display (`letter.isSigned()` / `getDisplayedSignedDate()`), **not** for detail history row `USER_SIGNED` |
| 20 | `documentSignature` | `DocumentSignature` | `signatures[0].signingDateTime`, `signatures[0].user.email/firstName/lastName` |

</div>
