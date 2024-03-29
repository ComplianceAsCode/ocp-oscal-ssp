---
x-trestle-comp-def-rules:
  OCP4:
    - name: api_server_api_priority_gate_enabled
      description: Ensure that the admission control plugin EventRateLimit is set
    - name: api_server_api_priority_flowschema_catch_all
      description: Ensure that the admission control plugin EventRateLimit is set
  OSCO:
    - name: api_server_api_priority_gate_enabled
      description: Ensure that the admission control plugin EventRateLimit is set
    - name: api_server_api_priority_flowschema_catch_all
      description: Ensure that the admission control plugin EventRateLimit is set
x-trestle-global:
  profile:
    title: OCP4 CIS Profile
    href: trestle://profiles/OCP4_CIS/profile.json
---

# CIS-1.2.10 - \[API Server\] Ensure that the APIPriorityAndFairness feature gate is enabled

## Control Statement

Limit the rate at which the API server accepts requests.

## Control rationale_statement

Using `EventRateLimit` admission control enforces a limit on the number of events that the API Server will accept in a given time slice. A misbehaving workload could overwhelm and DoS the API Server, making it unavailable. This particularly applies to a multi-tenant cluster, where there might be a small percentage of misbehaving tenants which could have a significant impact on the performance of the cluster overall. Hence, it is recommended to limit the rate of events that the API server will accept.

Note: This is an Alpha feature in the Kubernetes 1.15 release.

## Control impact_statement

None, as the OpenShift kubelet has been fixed to send fewer requests.

## Control remediation_procedure

No remediation is required.

## Control audit_procedure

EventRateLimit admission plugin cannot be enabled in Openshift. It was developed to alleviate the potential issue of flooding the API server with requests. However, the kubelet has since been fixed to send fewer events. OpenShift 4.5 and forward uses the api priority and fairness feature to limit the rate at which the API server accepts requests.

Run the following command:

```
#Verify the APIPriorityAndFairness feature-gate
oc get kubeapiservers.operator.openshift.io cluster -o json | jq '.spec.observedConfig.apiServerArguments'

#Verify the set of admission-plugins for OCP 4.6 and higher
oc -n openshift-kube-apiserver get configmap config -o json | jq -r '.data."config.yaml"' | jq '.apiServerArguments."enable-admission-plugins"'

#Check that no overrides are configured
oc get kubeapiservers.operator.openshift.io cluster -o json | jq -r '.spec.unsupportedConfigOverrides'
```

For 4.5, verify that the feature-gate is turned on for the APIServer priority and fairness: `APIPriorityAndFairness=true`. In OCP 4.5 and earlier, the default set of admission plugins are compiled into the `apiserver` and are not visible in the configuration yaml.

For 4.6 and above, verify that the set of admission plugins does not include `EventRateLimit`. 

Verify that no unsupported Overrides are configured.

## Control CIS_Controls

TITLE:Use of Secure Network Management and Communication Protocols  CONTROL:v8 12.6 DESCRIPTION:Use secure network management and communication protocols (e.g., 802.1X, Wi-Fi Protected Access 2 (WPA2) Enterprise or greater).;TITLE:Enable Operating System Anti-Exploitation Features/ Deploy Anti-Exploit Technologies CONTROL:v7 8.3 DESCRIPTION:Enable anti-exploitation features such as Data Execution Prevention (DEP) or Address Space Layout Randomization (ASLR) that are available in an operating system or deploy appropriate toolkits that can be configured to apply protection to a broader set of applications and executables.;

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

### This System

<!-- Add implementation prose for the main This System component for control: CIS-1.2.10 -->

#### Implementation Status: planned

### OCP4

<!-- Add control implementation description here for control: CIS-1.2.10 -->

#### Rules:

  - api_server_api_priority_gate_enabled
  - api_server_api_priority_flowschema_catch_all

#### Implementation Status: planned

### OSCO

<!-- Add control implementation description here for control: CIS-1.2.10 -->

#### Rules:

  - api_server_api_priority_gate_enabled
  - api_server_api_priority_flowschema_catch_all

#### Implementation Status: planned

______________________________________________________________________
