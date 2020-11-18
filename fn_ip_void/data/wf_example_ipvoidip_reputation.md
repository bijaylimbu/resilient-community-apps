<!--
    DO NOT MANUALLY EDIT THIS FILE
    THIS FILE IS AUTOMATICALLY GENERATED WITH resilient-circuits codegen
-->

# Example: IPVOID-IP Reputation

## Function - IPVOID API Implementation

### API Name
`fn_ip_void_request`

### Output Name
`None`

### Message Destination
`fn_ip_void`

### Pre-Processing Script
```python
inputs.ip_void_artifact_value = artifact.value
inputs.ip_void_artifact_type = artifact.type
```

### Post-Processing Script
```python
try:
    des = artifact.description.content
except Exception:
  des = None
  
if des is None:
 
  artifact.description = u"""IPVOID threat intelligence {0}""".format(results["data"])
else:

  artifact.description = des + u"""IPVOID threat intelligence {0}""".format(results["data"])
```

---
