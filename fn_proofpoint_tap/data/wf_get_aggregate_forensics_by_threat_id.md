<!--
    DO NOT MANUALLY EDIT THIS FILE
    THIS FILE IS AUTOMATICALLY GENERATED WITH resilient-circuits codegen
-->

# Exemple: Proofpoint TAP - Aggregate Forensics for Campaign


## Function - Proofpoint TAP Get Forensics

### API Name
`fn_pp_forensics`

### Output Name
`None`

### Message Destination
`fn_proofpoint_tap`

### Pre-Processing Script
```python
inputs.proofpoint_threat_id = artifact.value
inputs.proofpoint_aggregate_flag = True
```

### Post-Processing Script
```python
# results is a Dictionary and data is a List
if results and results.get("data"):
  incident.addNote(str(results.get("data")))
else:
  incident.addNote("No Forensics information found for artifact {}.".format(artifact.value))
```

---

