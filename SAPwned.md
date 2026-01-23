### 📑 SAPwned: Full Takeover of SAP AI Core

---

* **Severity:** 🔴 Critical
* **Researchers:** Hillai Ben-Sasson (Wiz Research)
* **Date Disclosed:** July 17, 2024
* **Target:** SAP AI Core (Shared Cloud Environment)
* **Impact:** Cross-tenant data access, supply-chain poisoning, and full Kubernetes cluster takeover.

---

### 📖 Executive Summary

"SAPwned" is a chain of five vulnerabilities discovered in **SAP AI Core**, a platform used for training and deploying AI models. Because AI platforms inherently allow users to run arbitrary code (for training models), isolation is critical. Wiz researchers bypassed these isolation layers, moved laterally through the internal network, and gained **Cluster Administrator** privileges.

This allowed them to access sensitive credentials (AWS, Azure, SAP HANA) and private AI artifacts belonging to other SAP customers.

---

### 🛠 The Vulnerability Chain (Step-by-Step)

#### 1. Initial Access: Arbitrary Code Execution

SAP AI Core allows users to provide an **Argo Workflow** file to spawn a Kubernetes Pod for AI training. By design, this allows the execution of user-controlled code.

#### 2. Bug #1: Bypassing Network Restrictions (Istio Bypass)

Initially, the Pod was restricted by an Istio sidecar proxy. However, researchers found they could bypass these rules using two specific Kubernetes configurations that the admission controller failed to block:

* **`shareProcessNamespace: true`**: Allowed the user container to see the Istio sidecar's processes and steal its access token.
* **`runAsUser: 1337`**: 1337 is the UID for the Istio proxy. Since Istio is excluded from its own traffic-blocking rules (to avoid infinite loops), running as this user granted unrestricted network access.

#### 3. Bug #2: Information Leak via Grafana Loki

With network access, researchers scanned the internal network and found a **Grafana Loki** instance.

* **Action:** Requested the `/config` endpoint.
* **Result:** The API leaked AWS secrets used for S3 bucket access, containing logs and metadata from various customer Pods.

#### 4. Bug #3: Unauthenticated AWS EFS Shares

The researchers discovered six **AWS Elastic File System (EFS)** instances listening on port 2049.

* **Action:** Used standard NFS tools to mount the shares.
* **Result:** Because the internal network was "trusted," no authentication was required. They gained read/write access to massive amounts of customer AI data, datasets, and code.

#### 5. Bug #4: Tiller (Helm v2) Exposure

The most critical discovery was an unauthenticated **Tiller server** (the server-side component of Helm v2) on port 44134.

* **Action:** Queried the Tiller gRPC interface.
* **Result:** Obtained secrets for SAP's **internal Docker Registry** and **Artifactory** server, enabling a potential supply-chain attack.

#### 6. Bug #5: Full Cluster Takeover

The Helm server allowed not just reading, but **writing**.

* **Action:** Deployed a malicious Helm chart designed to spawn a Pod with `cluster-admin` privileges.
* **Result:** Gained full control over the Kubernetes cluster, allowing them to read every secret in the namespace in plaintext.

---

### 💻 Proof of Concept (PoC) Snippets

#### Argo Workflow Entry Point

Users could trigger the vulnerability by submitting a configuration like this:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Workflow
spec:
  templates:
  - name: exploit-pod
    container:
      image: alpine
      command: ["sh", "-c", "sleep 3600"]
      securityContext:
        runAsUser: 1337 # Bypass Istio restrictions
    shareProcessNamespace: true # Access sidecar tokens

```

#### Extracting Helm Secrets

Once inside the network, the researchers could communicate with the Tiller server using the Helm client:

```bash
# Querying the unauthenticated Tiller server
helm --host <internal-tiller-ip>:44134 list

# Deploying a malicious chart for cluster-admin access
helm --host <internal-tiller-ip>:44134 install ./malicious-chart

```

#### Accessing Customer Secrets (Post-Takeover)

With `cluster-admin` rights, they could pull all secrets:

```bash
# Example command to view all secrets in the namespace
kubectl get secrets -o yaml

```

---

### 📉 Impact & Potential Exploitation

| Impact Category | Description |
| --- | --- |
| **Data Theft** | Access to models, training datasets, and proprietary code of other tenants. |
| **Credential Theft** | Exposure of customers' AWS, Azure, and SAP HANA cloud keys. |
| **Supply Chain** | Ability to modify Docker images in SAP’s registry to inject backdoors. |
| **Inference Manipulation** | Altering AI models to produce biased or incorrect outputs (Data Poisoning). |

---

### 🛡️ Remediation

SAP has since patched all reported vulnerabilities. Key fixes included:

1. **Hardened Admission Controllers:** Blocking the use of specific UIDs (like 1337) and `shareProcessNamespace`.
2. **Service Decommissioning:** Removing insecure components like Tiller (Helm v2).
3. **Network Hardening:** Implementing strict "Zero Trust" within the internal network so that internal services require authentication.
4. **Secret Rotation:** SAP rotated all potentially exposed infrastructure secrets.

---
