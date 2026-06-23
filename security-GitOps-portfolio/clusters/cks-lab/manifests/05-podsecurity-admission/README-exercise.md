[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/platform-Ubuntu%2022.04%2B-lightgrey)](#)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-MicroK8s%20%7C%20kubeadm-blue)](#)
[![YouTube](https://img.shields.io/badge/YouTube-TechShorts-red)](https://www.youtube.com/@adaribain)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Adari%20Bain-blue)](https://www.linkedin.com/in/adari-bain-298924152/)
    
    Kubernetes/gitops/
    ├── clusters/
    │   └── cks-lab/
    │       ├── applications/
    │       │   ├── 05-pod-security-admission.yaml
    │       │   ├── 05-psa-namespaces.yaml
    │       │   └── 05-psa-exemptions.yaml
    │       └── manifests/
    │           └── 05-podsecurity-admission/
    │               ├── namespaces/
    │               │   ├── privileged-ns.yaml
    │               │   ├── baseline-ns.yaml
    │               │   └── restricted-ns.yaml
    │               ├── test-workloads/
    │               │   ├── bad-workloads/
    │               │   │   ├── privileged-pod.yaml
    │               │   │   ├── hostpath-pod.yaml
    │               │   │   └── root-pod.yaml
    │               │   ├── good-workloads/
    │               │   │   ├── secure-deployment.yaml
    │               │   │   ├── secure-statefulset.yaml
    │               │   │   └── secure-daemonset.yaml
    │               │   └── test-results.yaml
    │               ├── exemptions/
    │               │   ├── service-account-exemption.yaml
    │               │   ├── namespace-exemption.yaml
    │               │   └── runtimeclass-exemption.yaml
    │               ├── monitoring/
    │               │   ├── psa-audit-job.yaml
    │               │   └── violation-dashboard.yaml
    │               └── README-exercise.md
    ├── policies/
    │   └── psa-configuration/
    │       ├── psa-config.yaml
    │       ├── admission-config.yaml
    │       └── webhook-config.yaml
    └── tools/
        └── psa-validator/
            ├── validate-psa.sh
            └── psa-report.yaml

markdown
# CKS Exercise 05: Pod Security Admission (PSA)

## ⚠️ IMPORTANT:
**PSA replaces PodSecurityPolicies (PSP) in Kubernetes 1.25+**
#### This is the **modern, built-in** Kubernetes security standard.

## Learning Objectives:
#### - Understand PSA labels and enforcement modes
#### - Implement namespace-level security policies
#### - Configure exemptions for system workloads
#### - Migrate from PSP to PSA

## Tasks:

### Task 1: Deploy PSA Test Environment
#### 1. Sync the PSA applications
#### 2. Verify namespaces are created with correct labels
#### 3. Check PSA admission controller is working

### Task 2: Test Enforcement Levels
#### For each namespace (privileged, baseline, restricted):
#### 1. Deploy test workloads
#### 2. Observe PSA behavior (allow/warn/deny)
#### 3. Document results

### Task 3: Implement Gradual Enforcement
#### Design a 4-week rollout plan:
#### Week 1: Add 'warn' labels to all namespaces
#### Week 2: Add 'audit' labels, analyze violations
#### Week 3: Fix violations in development namespaces
#### Week 4: Apply 'enforce' to development, continue audit in production

text

### Task 4: Create Exemption Process
#### 1. Document exemption requirements
#### 2. Create exemption request template
#### 3. Implement compensating controls
#### 4. Set up review process

## Success Criteria:
#### - [ ] PSA namespaces deployed via GitOps
#### - [ ] Enforcement tested and documented
#### - [ ] Gradual rollout plan created
#### - [ ] Exemption process documented
#### - [ ] Monitoring and alerting configured

## Production Checklist:
#### - [ ] All namespaces have PSA labels
#### - [ ] System namespaces exempted
#### - [ ] Audit logging enabled
#### - [ ] Regular compliance reports
#### - [ ] Violation alerting configured
#### 🚀 DEPLOYMENT WORKFLOW:
#### In ArgoCD UI:
#### text
    Application 1: psa-namespaces
    Path: gitops/clusters/cks-lab/manifests/05-podsecurity-admission/namespaces

    Application 2: psa-test-workloads  
    Path: gitops/clusters/cks-lab/manifests/05-podsecurity-admission/test-workloads
    Namespace: psa-lab
#### Testing Commands:
#### bash
# Test PSA enforcement
    kubectl apply -f bad-workloads/privileged-pod.yaml -n psa-restricted  # Should fail
    kubectl apply -f good-workloads/secure-deployment.yaml -n psa-restricted  # Should succeed

# Check namespace labels
    kubectl get ns --show-labels | grep pod-security

# View PSA warnings
    kubectl get events -n psa-restricted --field-selector reason=PodSecurity
#### 💡 KEY PSA CONCEPTS:
#### Namespace-scoped: Policies apply per namespace, not cluster-wide

#### Three modes: enforce, audit, warn (gradual rollout)

#### Three levels: privileged, baseline, restricted

#### Built-in: No external webhooks needed (Kubernetes 1.22+)

#### Migration path: PSP → PSA with backward compatibility

#### 🔧 PSA vs PSP COMPARISON:
#### Feature	PodSecurityPolicies (PSP)	Pod Security Admission (PSA)
#### Scope	Cluster-wide	Namespace-level
#### Complexity	High (RBAC binding)	Low (labels)
#### Built-in	No (deprecated in 1.25)	Yes (1.22+)
#### Adoption	Declining	Recommended
#### Learning Curve	Steep	Gentle
#### Want me to:

#### Add PSP-to-PSA migration guide?

#### Create compliance dashboard with Grafana?

#### Add CI/CD pipeline for PSA validation?

#### Create exemption request workflow?

#### This transforms PSA lab into a complete production implementation guide for the modern Kubernetes security standard!

