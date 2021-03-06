<!--
    DO NOT MANUALLY EDIT THIS FILE
    THIS FILE IS AUTOMATICALLY GENERATED WITH resilient-circuits codegen
-->

# Example: Exchange Online Query Messages on Artifact

## Function - Exchange Online: Query Messages

### API Name
`exchange_online_query_emails`

### Output Name
`None`

### Message Destination
`fn_exchange_online`

### Pre-Processing Script
```python
# Get the email address of the user whose mailbox will be queried.
inputs.exo_email_address = artifact.value

# Get the search criteria from the activity rules if available. 
inputs.exo_email_address_sender = inputs.exo_email_address_sender if rule.properties.exo_email_address_sender is None else rule.properties.exo_email_address_sender
inputs.exo_mail_folders         = inputs.exo_mail_folders         if rule.properties.exo_mailfolder_id        is None else rule.properties.exo_mailfolder_id
inputs.exo_message_subject      = inputs.exo_message_subject      if rule.properties.exo_message_subject      is None else rule.properties.exo_message_subject
inputs.exo_message_body         = inputs.exo_message_body         if rule.properties.exo_message_body         is None else rule.properties.exo_message_body
inputs.exo_start_date           = inputs.exo_start_date           if rule.properties.exo_start_date           is None else rule.properties.exo_start_date
inputs.exo_end_date             = inputs.exo_end_date             if rule.properties.exo_end_date             is None else rule.properties.exo_end_date
inputs.exo_has_attachments      = inputs.exo_has_attachments      if rule.properties.exo_has_attachments      is None else rule.properties.exo_has_attachments

```

### Post-Processing Script
```python
from java.util import Date

note = u"Exchange Online Query user:\n"
note_len = len(note)

# Add each email as a row in the query results data table
for user in results["content"]:
  # If an email address is not found post to a note.
  if user["status_code"] == 404:
    line = u"email address not found: {}\n".format(user["email_address"])
    note = note + line
    
  for email in user["email_list"]:
    message_row = incident.addRow("exo_message_query_results_dt")
    message_row.exo_dt_query_date = Date()
    message_row.exo_dt_message_id = email.id
    message_row.exo_dt_received_date   = email.receivedDateTime
    message_row.exo_dt_email_address = user["email_address"]
    if email.sender:
      message_row.exo_dt_sender_email = email.sender.emailAddress.address
    else:
      message_row.exo_dt_sender_email = ""
    message_row.exo_dt_message_subject = email.subject
    message_row.exo_dt_has_attachments = email.hasAttachments
    if email.webLink:
      ref_html = u"""<a href='{0}'>Link</a>""".format(email.webLink)
      message_row.exo_dt_web_link = helper.createRichText(ref_html)
    else:
      message_row.exo_dt_web_link = ""
 
    message_row.exo_dt_status = helper.createRichText("Active")

# If any email addresses where not found post a note
if len(note) > note_len:
  incident.addNote(note)
```

---

