---
x-trestle-comp-def-rules:
  OCP4:
    - name: rbac_wildcard_use
      description: Minimize wildcard use in Roles and ClusterRoles (info)
  OSCO:
    - name: rbac_wildcard_use
      description: Minimize wildcard use in Roles and ClusterRoles (info)
x-trestle-global:
  profile:
    title: OCP4 CIS Profile
    href: trestle://profiles/OCP4_CIS/profile.json
---

# CIS-5.1.3 - \[RBAC and Service Accounts\] Minimize wildcard use in Roles and ClusterRoles

## Control Statement

Kubernetes Roles and ClusterRoles provide access to resources based on sets of objects and actions that can be taken on those objects. It is possible to set either of these to be the wildcard "*" which matches all items.     Use of wildcards is not optimal from a security perspective as it may allow for inadvertent access to be granted when new resources are added to the Kubernetes API either as CRDs or in later versions of the product.

## Control rationale_statement

The principle of least privilege recommends that users are provided only the access required for their role and nothing more. The use of wildcard rights grants is likely to provide excessive rights to the Kubernetes API.

## Control remediation_procedure

Where possible replace any use of wildcards in clusterroles and roles with specific objects or actions.

## Control audit_procedure

Retrieve the roles defined across each namespaces in the cluster and review for wildcards

```

oc get roles --all-namespaces -o yaml

for i in $(oc get roles -A -o jsonpath='{.items[*].metadata.name}'); do oc describe clusterrole ${i}; done
```

Retrieve the cluster roles defined in the cluster and review for wildcards

```
oc get clusterroles -o yaml

for i in $(oc get clusterroles -o jsonpath='{.items[*].metadata.name}'); do oc describe clusterrole ${i}; done
```

## Control CIS_Controls

TITLE:Define and Maintain Role-Based Access Control CONTROL:v8 6.8 DESCRIPTION:Define and maintain role-based access control, through determining and documenting the access rights necessary for each role within the enterprise to successfully carry out its assigned duties. Perform access control reviews of enterprise assets to validate that all privileges are authorized, on a recurring schedule at a minimum annually, or more frequently.;

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

### This System

<!-- Add implementation prose for the main This System component for control: CIS-5.1.3 -->

#### Implementation Status: planned

### OCP4

<!-- Add control implementation description here for control: CIS-5.1.3 -->

#### Rules:

  - rbac_wildcard_use

#### Implementation Status: planned

### OSCO

<!-- Add control implementation description here for control: CIS-5.1.3 -->

#### Rules:

  - rbac_wildcard_use

#### Implementation Status: planned

______________________________________________________________________
