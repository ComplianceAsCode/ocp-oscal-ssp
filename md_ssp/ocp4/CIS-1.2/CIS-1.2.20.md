---
x-trestle-comp-def-rules:
  OCP4:
    - name: api_server_bind_address
      description: Ensure that the --secure-port argument is not set to 0
  OSCO:
    - name: api_server_bind_address
      description: Ensure that the --secure-port argument is not set to 0
x-trestle-global:
  profile:
    title: OCP4 CIS Profile
    href: trestle://profiles/OCP4_CIS/profile.json
---

# CIS-1.2.20 - \[API Server\] Ensure that the --secure-port argument is not set to 0

## Control Statement

Do not disable the secure port.

## Control rationale_statement

The secure port is used to serve https with authentication and authorization. If you disable it, no https traffic is served and all traffic is served unencrypted.

## Control impact_statement

You need to set the API Server up with the right TLS certificates.

## Control remediation_procedure

None required.

## Control audit_procedure

The `openshift-kube-apiserver` is served over HTTPS with authentication and authorization; the secure API endpoint is bound to `0.0.0.0:6443` by `default`. In OpenShift, the only supported way to access the API server pod is through the load balancer and then through the internal service. The value is set by the `bindAddress` argument under the `servingInfo` parameter.

Run the following command:

```
oc get kubeapiservers.operator.openshift.io cluster -o json | jq '.spec.observedConfig'

# Should return only 6443
oc get pods -n openshift-kube-apiserver -l app=openshift-kube-apiserver -o jsonpath='{.items[*].spec.containers[?(@.name=="kube-apiserver")].ports[*].containerPort}'
```

Verify that the `--bindAddress` argument is set `0.0.0.0:6443`.

Verify that the only port returned is `6443`.

## Control CIS_Controls

TITLE:Encrypt Sensitive Data in Transit CONTROL:v8 3.10 DESCRIPTION:Encrypt sensitive data in transit. Example implementations can include: Transport Layer Security (TLS) and Open Secure Shell (OpenSSH).;TITLE:Encrypt All Sensitive Information in Transit CONTROL:v7 14.4 DESCRIPTION:Encrypt all sensitive information in transit.;

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

### This System

<!-- Add implementation prose for the main This System component for control: CIS-1.2.20 -->

#### Implementation Status: planned

### OCP4

<!-- Add control implementation description here for control: CIS-1.2.20 -->

#### Rules:

  - api_server_bind_address

#### Implementation Status: planned

### OSCO

<!-- Add control implementation description here for control: CIS-1.2.20 -->

#### Rules:

  - api_server_bind_address

#### Implementation Status: planned

______________________________________________________________________
