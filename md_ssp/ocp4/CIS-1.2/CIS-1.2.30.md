---
x-trestle-comp-def-rules:
  OCP4:
    - name: api_server_tls_cert
      description: Ensure that the --tls-cert-file and --tls-private-key-file arguments
        are set as appropriate
    - name: api_server_tls_private_key
      description: Ensure that the --tls-cert-file and --tls-private-key-file arguments
        are set as appropriate
  OSCO:
    - name: api_server_tls_cert
      description: Ensure that the --tls-cert-file and --tls-private-key-file arguments
        are set as appropriate
    - name: api_server_tls_private_key
      description: Ensure that the --tls-cert-file and --tls-private-key-file arguments
        are set as appropriate
x-trestle-global:
  profile:
    title: OCP4 CIS Profile
    href: trestle://profiles/OCP4_CIS/profile.json
---

# CIS-1.2.30 - \[API Server\] Ensure that the --tls-cert-file and --tls-private-key-file arguments are set as appropriate

## Control Statement

Setup TLS connection on the API server.

## Control rationale_statement

API server communication contains sensitive parameters that should remain encrypted in transit. Configure the API server to serve only HTTPS traffic.

## Control impact_statement

TLS and client certificate authentication must be configured for your Kubernetes cluster deployment. By default, OpenShift uses X.509 certificates to provide secure connections between the API server and node/kubelet. OpenShift Container Platform monitors certificates for proper validity, for the cluster certificates it issues and manages. The OpenShift Container Platform manages certificate rotation and the alerting framework has rules to help identify when a certificate issue is about to occur.

## Control remediation_procedure

OpenShift automatically manages TLS authentication for the API server communication with the node/kublet. This is not configurable. 

You may optionally set a custom default certificate to be used by the API server when serving content in order to enable clients to access the API server at a different host name or without the need to distribute the cluster-managed certificate authority (CA) certificates to the clients. Follow the directions in the OpenShift documentation

[User-provided certificates for the API server](
https://docs.openshift.com/container-platform/4.5/security/certificate-types-descriptions.html#user-provided-certificates-for-the-api-server_ocp-certificates)

## Control audit_procedure

OpenShift uses X.509 certificates to provide secure connections between API server and node/kubelet by default. OpenShift does not use values assigned to the `tls-cert-file` or `tls-private-key-file` flags. OpenShift generates the certificate files and sets the arguments appropriately. 

The API server is accessible by clients external to the cluster at `api.<cluster_name>.<base_domain>`. The administrator must set a custom default certificate to be used by the API server when serving content in order to enable clients to access the API server at a different host name or without the need to distribute the cluster-managed certificate authority (CA) certificates to the clients. 

Run the following command:

```
# TLS Cert File - openshift-kube-apiserver
oc get configmap config -n openshift-kube-apiserver -ojson | \
 jq -r '.data["config.yaml"]' | \
 jq -r .servingInfo.certFile

# TLS Key File
oc get configmap config -n openshift-kube-apiserver -ojson | \
 jq -r '.data["config.yaml"]' | \
 jq -r .servingInfo.keyFile
```

Verify that the following files exist.

`/etc/kubernetes/static-pod-certs/secrets/service-network-serving-certkey/tls.crt` 
`/etc/kubernetes/static-pod-certs/secrets/service-network-serving-certkey/tls.key`

## Control CIS_Controls

TITLE:Encrypt Sensitive Data in Transit CONTROL:v8 3.10 DESCRIPTION:Encrypt sensitive data in transit. Example implementations can include: Transport Layer Security (TLS) and Open Secure Shell (OpenSSH).;TITLE:Encrypt All Sensitive Information in Transit CONTROL:v7 14.4 DESCRIPTION:Encrypt all sensitive information in transit.;

______________________________________________________________________

## What is the solution and how is it implemented?

<!-- For implementation status enter one of: implemented, partial, planned, alternative, not-applicable -->

<!-- Note that the list of rules under ### Rules: is read-only and changes will not be captured after assembly to JSON -->

### This System

<!-- Add implementation prose for the main This System component for control: CIS-1.2.30 -->

#### Implementation Status: planned

### OCP4

<!-- Add control implementation description here for control: CIS-1.2.30 -->

#### Rules:

  - api_server_tls_cert
  - api_server_tls_private_key

#### Implementation Status: planned

### OSCO

<!-- Add control implementation description here for control: CIS-1.2.30 -->

#### Rules:

  - api_server_tls_cert
  - api_server_tls_private_key

#### Implementation Status: planned

______________________________________________________________________
