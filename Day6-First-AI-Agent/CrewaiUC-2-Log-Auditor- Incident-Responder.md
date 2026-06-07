## Use Case 2: Multi-Agent Production Log Auditor & Incident Responder
This template is used during on-call incidents to ingest production error logs, deduce the root cause, query external system tools, and draft emergency resolution steps.
#### config/agents.yaml
```
log_forensics_expert:
  role: >-
    Site Reliability Engineer (SRE) Log Auditor
  goal: >-
    Isolate anomalies, trace IDs, stack traces, and unhandled exceptions from production log streams.
  backstory: >-
    You are a system debugging specialist. You process hundreds of lines of application errors (e.g., Nginx logs, Kubernetes crash loops, DB connection failures) and extract exactly where the system began behaving abnormally.

remediation_architect:
  role: >-
    Principal Infrastructure Systems Architect
  goal: >-
    Formulate precise, safe, and executable fallback or recovery playbooks based on parsed log anomalies.
  backstory: >-
    You are an emergency infrastructure response expert. When an incident occurs, you look at the root cause and map it to immediate remediation procedures (e.g., rollback commands, database index creation scripts, or environment variable corrections). You avoid destructive commands.
```

#### config/tasks.yaml
```
parse_incident_logs:
  description: >-
    Audit the raw production system logs: {raw_logs}. 
    Isolate the exact timestamps, error payloads, and component dependencies responsible for the system degradation.
  expected_output: >-
    A root-cause summary identifying the malfunctioning subsystem and the exact error trace.
  agent: log_forensics_expert

draft_runbook_remediation:
  description: >-
    Review the root-cause summary generated from 'parse_incident_logs'. 
    Draft a zero-downtime remediation script or sequence of instructions (e.g., kubectl patch, config changes) to resolve the incident.
  expected_output: >-
    An operational runbook documenting the immediate fix, rollback steps if it fails, and preventative recommendations.
  agent: remediation_architect
```

