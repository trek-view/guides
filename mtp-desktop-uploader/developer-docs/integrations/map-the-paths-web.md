# Map the Paths Web

### **Setup**

If MTPW variables stored in `.env` file, all sequence data can be synced to MTPW.

If user selects MTPW integration, [Mapillary integration must also be selected by the user.](mapillary.md) When user selects either Mapillary or MTPW integration other option will be marked.

### **Workflow**

![MTPDU to MTPW sync](../../../.gitbook/assets/mapillary-sync-ui-4-.jpg)

1. User authenticates to MTPW
   1. This is done on user opening app and key stored
2. User completes sequences creation
   1. With MTPW/Mapillary selected and authentication completed
3. App sends sequence data \(name, description, transport type and tags\) to MTPW
4. MTPW responds with sequence ID
5. [Mapillary integration workflow starts](mapillary.md)

[View the full MTPW API Docs here.](https://documenter.getpostman.com/view/10024679/TVK5e2fn)

[For reference, you can see the way the Sequence is then created on Map the Paths web here.](../../../mtp-web/developer-docs/functions/sequences.md)



