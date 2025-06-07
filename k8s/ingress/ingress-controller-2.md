An **Ingress ALB (Application Load Balancer)** in Kubernetes, especially in AWS EKS (Elastic Kubernetes Service), works by integrating the Kubernetes **Ingress resource** with the AWS **ALB** to route external HTTP/HTTPS traffic into your Kubernetes cluster. Here's a detailed breakdown of how it works:

---

## 🧭 Key Components

### 1. **Kubernetes Ingress**

* A Kubernetes resource that defines **rules for routing** external traffic to internal services.
* Example:

  ```yaml
  apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
    name: my-ingress
    annotations:
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

### 2. **AWS ALB Ingress Controller (Now AWS Load Balancer Controller)**

* A Kubernetes controller that:

  * Watches for `Ingress` resources.
  * Provisions and configures an **ALB** in AWS.
  * Updates ALB listeners and rules based on the ingress spec.

---

## 🔁 How It Works (Step-by-Step)

### 1. **Deployment of Ingress Controller**

* You install the **AWS Load Balancer Controller** in your cluster.
* It requires IAM permissions and an OIDC provider to manage AWS resources like ALBs.

### 2. **Creation of an Ingress Resource**

* You define an Ingress YAML that maps domain paths to Kubernetes services.
* Annotations control ALB-specific features (e.g., SSL certs, WAF, target groups).

### 3. **Controller Watches and Provisions**

* The Ingress Controller:

  * Detects the new Ingress.
  * Creates an **ALB** (or modifies an existing one).
  * Creates **Listeners** for HTTP/HTTPS ports (80/443).
  * Sets up **Target Groups** for each backend Kubernetes service.
  * Registers **EC2 nodes or IPs (via awsvpc mode)** as targets.

### 4. **Routing Mechanism**

* When a request hits the ALB:

  * It matches the domain and path against **Listener Rules**.
  * The ALB routes the request to the corresponding **Target Group**.
  * The target group forwards traffic to the appropriate **Kubernetes pod**.

---

## 🔒 SSL Termination (TLS)

* You can terminate SSL at the ALB using ACM certificates:

  ```yaml
  alb.ingress.kubernetes.io/certificate-arn: arn:aws:acm:region:account:certificate/xyz
  alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80, "HTTPS":443}]'
  ```

---

## 📌 Example Annotations

| Annotation                                   | Purpose                         |
| -------------------------------------------- | ------------------------------- |
| `alb.ingress.kubernetes.io/scheme`           | `internet-facing` or `internal` |
| `alb.ingress.kubernetes.io/certificate-arn`  | ACM cert for HTTPS              |
| `alb.ingress.kubernetes.io/target-type`      | `ip` or `instance`              |
| `alb.ingress.kubernetes.io/waf-acl-id`       | Attach AWS WAF                  |
| `alb.ingress.kubernetes.io/healthcheck-path` | Custom health check             |

---

## 🛡 Security

* **Security Groups**: Control access to the ALB and backend services.
* **WAF**: Attach AWS WAF to protect against common web attacks.
* **Private ALBs**: Use for internal applications (with `internal` scheme).

---

## ⚙ Target Type

* `instance`: Routes to EC2 nodes (typically with `hostPort`).
* `ip`: Routes directly to pod IPs (requires `awsvpc` networking).

---

## 📊 Monitoring & Logging

* Enable **access logs** for ALB to S3.
* Monitor with **CloudWatch metrics** and **X-Ray tracing**.

---

Let me know if you want a full working YAML example with TLS, WAF, and logging annotations.
