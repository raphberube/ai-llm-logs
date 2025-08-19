# mcp-atlassian

GitHub repo of the `mcp-atlassian` MCP server : https://github.com/sooperset/mcp-atlassian

## 1. Start the MCP server

Start the MCP server in parallel (and before) starting Claude Code.

```bash
docker run --rm -p 9000:9000 \
  -e JIRA_URL="$(~/get-key.sh $GET_KEY_ID_JIRA URL)" \
  -e MCP_VERY_VERBOSE="true" \
  -e JIRA_USERNAME="$(~/get-key.sh $GET_KEY_ID_JIRA UserName)" \
  -e JIRA_API_TOKEN="$(~/get-key.sh $GET_KEY_ID_JIRA)" \
  ghcr.io/sooperset/mcp-atlassian@sha256:27c8e5b890e16134a443f49683eaa794fdb48ed5bef785c1b4c88a0cd729df56 \
  --transport streamable-http --port 9000 --enabled-tools "jira_get_user_profile,jira_search_fields,jira_search,jira_get_issue,jira_get_transitions,jira_get_sprints_from_board,jira_get_sprint_issues,jira_get_issue_link_types,jira_create_issue,jira_update_issue,jira_batch_create_issues,jira_add_comment,jira_transition_issue,jira_add_worklog,jira_create_issue_link"
```

## 2. Add the MCP Atlassian tool to Claude MCP

Start the MCP server in parallel (and before starting) of Claude Code.

```bash
claude mcp add --transport http mcp-atlassian http://localhost:9000/mcp --scope user
```

mcp-atlassian - jira_search_fields (MCP)(keyword: "original estimate", limit: 10)
  ⎿  [                                                                                                                                                                                                                
       {
         "id": "aggregatetimeoriginalestimate",
         "key": "aggregatetimeoriginalestimate",
         "name": "Σ Original Estimate",
         "custom": false,
         "orderable": false,
         "navigable": true,
         "searchable": false,
         "clauseNames": [],
         "schema": {
           "type": "number",
           "system": "aggregatetimeoriginalestimate"
         }
       },
       {
         "id": "timeoriginalestimate",
         "key": "timeoriginalestimate",
         "name": "Original estimate",
         "custom": false,
         "orderable": false,
         "navigable": true,
         "searchable": false,
         "clauseNames": [
           "originalEstimate",
     "timeoriginalestimate"
         ],
         "schema": {
           "type": "number",
           "system": "timeoriginalestimate"
         }
       },
       {
         "id": "customfield_10957",
         "key": "customfield_10957",
         "name": "UX Estimate",
         "untranslatedName": "UX Estimate",
         "custom": true,
         "orderable": true,
         "navigable": true,
         "searchable": true,
         "clauseNames": [
           "UX Estimate[Number]",
           "cf[10957]",
     "UX Estimate"
         ],
         "schema": {
           "type": "number",
           "custom": "com.atlassian.jira.plugin.system.customfieldtypes:float",
           "customId": 10957
         }
       },
       {
         "id": "customfield_11203",
         "key": "customfield_11203",
         "name": "date",
         "untranslatedName": "date",
         "custom": true,
         "orderable": true,
         "navigable": true,
         "searchable": true,
         "clauseNames": [
           "date",
           "cf[11203]",
     "date[Date]"
         ],
         "schema": {
           "type": "date",
           "custom": "com.atlassian.jira.plugin.system.customfieldtypes:datepicker",
           "customId": 11203
         }
       },
       {
         "id": "timeestimate",
         "key": "timeestimate",
         "name": "Remaining Estimate",
         "custom": false,
         "orderable": false,
         "navigable": true,
         "searchable": false,
         "clauseNames": [
           "remainingEstimate",
     "timeestimate"
         ],
         "schema": {
           "type": "number",
           "system": "timeestimate"
         }
       },
       {
         "id": "customfield_10696",
         "key": "customfield_10696",
         "name": "Technical Estimate",
         "untranslatedName": "Technical Estimate",
         "custom": true,
         "orderable": true,
         "navigable": true,
         "searchable": true,
         "clauseNames": [
           "cf[10696]",
           "Technical Estimate",
     "Technical Estimate[Paragraph]"
         ],
         "schema": {
           "type": "string",
           "custom": "com.atlassian.jira.plugin.system.customfieldtypes:textarea",
           "customId": 10696
         }
       },
       {
         "id": "customfield_10016",
         "key": "customfield_10016",
         "name": "Story point estimate",
         "untranslatedName": "Story point estimate",
         "custom": true,
         "orderable": true,
         "navigable": true,
         "searchable": true,
         "clauseNames": [
           "cf[10016]",
     "Story point estimate"
         ],
         "schema": {
           "type": "number",
           "custom": "com.pyxis.greenhopper.jira:jsw-story-points",
           "customId": 10016
         }
       },
       {
         "id": "customfield_10813",
         "key": "customfield_10813",
         "name": "UI/UX Story Points Estimate",
         "untranslatedName": "UI/UX Story Points Estimate",
         "custom": true,
         "orderable": true,
         "navigable": true,
         "searchable": true,
         "clauseNames": [
           "UI/UX Story Points Estimate[Number]",
           "UI/UX Story Points Estimate",
     "cf[10813]"
         ],
         "schema": {
           "type": "number",
           "custom": "com.atlassian.jira.plugin.system.customfieldtypes:float",
           "customId": 10813
         }
       },
       {
         "id": "aggregatetimeestimate",
         "key": "aggregatetimeestimate",
         "name": "Σ Remaining Estimate",
         "custom": false,
         "orderable": false,
         "navigable": true,
         "searchable": false,
         "clauseNames": [],
         "schema": {
           "type": "number",
           "system": "aggregatetimeestimate"
         }
       },
       {
         "id": "customfield_11651",
         "key": "customfield_11651",
         "name": "Backend Story Points Estimate",
         "untranslatedName": "Backend Story Points Estimate",
         "custom": true,
         "orderable": true,
         "navigable": true,
         "searchable": true,
         "clauseNames": [
           "cf[11651]",
           "Backend Story Points Estimate[Number]",
     "Backend Story Points Estimate"
         ],
         "schema": {
           "type": "number",
           "custom": "com.atlassian.jira.plugin.system.customfieldtypes:float",
           "customId": 11651
         }
       }
     ]