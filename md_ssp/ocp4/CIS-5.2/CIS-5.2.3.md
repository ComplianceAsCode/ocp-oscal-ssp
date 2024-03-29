---
x-trestle-comp-def-rules:
  OCP4:
    - name: scc_limit_ipc_namespace
      description: Minimize the admission of containers wishing to share the host
        IPC namespace (info)
  OSCO:
    - name: scc_limit_ipc_namespace
      description: Minimize the admission of containers wishing to share the host
        IPC namespace (info)
x-trestle-global:
  profile:
    title: OCP4 CIS Profile
    href: trestle://profiles/OCP4_CIS/profile.json
---

# CIS-5.2.3 - \[Pod Security Policies\] Minimize the admission of containers wishing to share the host IPC namespace

## Control Statement

Do not generally permit containers to be run with the `hostIPC` flag set to true.

## Control rationale_statement

A container running in the host's IPC namespace can use IPC to interact with processes outside the container.

There should be at least one Security Context Constraint (SCC) defined which does not permit containers to share the host IPC namespace.

If you have a requirement to containers which require hostIPC, this should be defined in a separate SCC and you should carefully check RBAC controls to ensure that only limited service accounts and users are given permission to access that SCC.

## Control impact_statement

Pods defined with `Allow Host IPC: true` will not be permitted unless they are run under a specific SCC.

## Control remediation_procedure

Create a SCC as described in the OpenShift documentation, ensuring that the `Allow Host IPC` field is set to `false`.

## Control audit_procedure

Get the set of SCCs with the following command:

```
oc get scc
```

For each SCC, check whether `Allow Host IPC` is enabled:

```
for i in `oc get scc --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}'`; do echo "$i"; oc describe scc $i | grep "Allow Host IPC"; done
```

Verify that there is at least one SCC which does not return true.

## Control CIS_Controls

TITLE:Perform Application Layer Filtering CONTROL:v8 13.10 DESCRIPTION:Perform application layer filtering. Example implementations include a filtering proxy, application layer firewall, or gateway.;TITLE:Deploy Application Layer Filtering Proxy Server CONTROL:v7 12.9 DESCRIPTION:Ensure that all network traffic to or from the Internet passes through an authenticated application layer proxy that is configured to filter unauthorized connections.;

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

### This System

<!-- Add implementation prose for the main This System component for control: CIS-5.2.3 -->

#### Implementation Status: planned

### OCP4

<!-- Add control implementation description here for control: CIS-5.2.3 -->

#### Rules:

  - scc_limit_ipc_namespace

#### Implementation Status: planned

### OSCO

<!-- Add control implementation description here for control: CIS-5.2.3 -->

#### Rules:

  - scc_limit_ipc_namespace

#### Implementation Status: planned

______________________________________________________________________
