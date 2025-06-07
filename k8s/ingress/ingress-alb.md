![image](https://github.com/user-attachments/assets/2e1b5041-ba16-447a-9327-34e081c250e3)


In **Amazon EKS (Elastic Kubernetes Service)**, **ALB** (Application Load Balancer) and **Ingress** work together via the **AWS Load Balancer Controller** to expose and route traffic from the internet to your Kubernetes services. Here's a full breakdown:

---

## 🧭 What Each Component Does

| Component                        | Role                                                                                |
| -------------------------------- | ----------------------------------------------------------------------------------- |
| **Ingress** (Kubernetes)         | A resource that defines HTTP/S routing rules for services                           |
| **AWS ALB**                      | A Layer 7 Load Balancer that routes HTTP/S traffic to targets                       |
| **AWS Load Balancer Controller** | A Kubernetes controller that provisions and manages ALBs based on Ingress resources |

---

## 🔁 How They Work Together – Step-by-Step

### 1. **Install the AWS Load Balancer Controller**

* This controller watches your Kubernetes cluster for **Ingress** resources with the `kubernetes.io/ingress.class: alb` annotation.
* You must associate an **IAM Role with proper permissions** using **IAM for Service Accounts (IRSA)**.

---

### 2. **Create an Ingress Resource**

You define an `Ingress` YAML with routing rules. Example:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
  annotations:
    kubernetes.io/ingress.class: alb
    alb.ingress.kubernetes.io/scheme: internet-facing
spec:
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /api
        pathType: Prefix
        backend:
          service:
            name: my-service
            port:
              number: 80
```

---

### 3. **Controller Provisions an ALB**

Once the Ingress is created:

* The controller:

  * Provisions an **ALB** in AWS.
  * Creates **Listeners** (on port 80/443).
  * Sets up **Listener Rules** to match hosts and paths.
  * Creates **Target Groups** that point to your EKS pods.
* It then configures **health checks**, **SSL**, **WAF**, and more based on annotations.

---

### 4. **Traffic Flow**

```text
Internet (HTTPS)
      |
      v
AWS ALB (Layer 7)
      |
      v
Ingress Controller (Listener Rules)
      |
      v
Target Group -> Pod IP (via ENIs with awsvpc mode)
```

### Example Routing:

* `myapp.example.com/api` → forwards to `my-service` on port 80.

---

## 🔌 Networking Considerations

* ALB communicates **directly with Pods** when using `target-type: ip` (requires `awsvpc` networking mode).
* Your **pods get ENIs** and IPs from your VPC, making them directly routable.
* Ensure **subnets** for the ALB and EKS worker nodes are configured with proper routing and security groups.

---

## 🧠 Summary: Who Does What?

| Component                    | Responsibility                                                |
| ---------------------------- | ------------------------------------------------------------- |
| You                          | Write Ingress + annotations                                   |
| Ingress                      | Defines routing rules (host/path to service)                  |
| AWS Load Balancer Controller | Converts Ingress → ALB + Listeners + Rules + Target Groups    |
| ALB                          | Routes traffic to targets (your pods) based on Ingress config |
| Target Groups                | Manage health checks and routing to pod IPs                   |

---

## 📘 Use Cases Enabled

✅ Host/path-based routing
✅ SSL Termination with ACM
✅ WAF integration
✅ Access logs to S3
✅ External (internet-facing) or internal ALBs
✅ Autoscaling and fault-tolerant routing

---

Would you like a **diagram** to visualize this flow, or a **Terraform + Helm combo** to provision the controller + Ingress automatically?
