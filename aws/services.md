Here's a visually enhanced **table of AWS services** with emblems (✅ for Regional, 🌐 for Global, 🔁 for Mixed), showing their **scope** and a short description:

---

### 🌍 AWS Service Scope Table with Emblems

| **Service**                     | **Scope** | **Emblem** | **Description**                                          |
| ------------------------------- | --------- | ---------- | -------------------------------------------------------- |
| ALB (Application Load Balancer) | Regional  | ✅          | Runs in a specific VPC and region                        |
| Route 53                        | Global    | 🌐         | Global DNS service with regional integrations            |
| IAM                             | Global    | 🌐         | Identity management shared across regions                |
| S3 (Simple Storage Service)     | Regional  | ✅          | Buckets are region-scoped but globally accessible        |
| Kinesis                         | Regional  | ✅          | Streams and analytics are created per region             |
| ECS (Elastic Container Service) | Regional  | ✅          | Clusters and services run in a chosen region             |
| DynamoDB                        | Regional  | ✅          | Region-scoped by default; Global Tables available        |
| RDS                             | Regional  | ✅          | Databases deployed in regional AZs                       |
| Aurora                          | Regional  | ✅          | Region-scoped RDS engine with multi-AZ support           |
| Subnets                         | Per AZ    | ✅          | Exist in individual availability zones                   |
| VPC                             | Regional  | ✅          | Virtual networks bound to a single region                |
| NACL (Network ACL)              | Regional  | ✅          | Linked to subnets in regional VPCs                       |
| Glacier (S3 Glacier)            | Regional  | ✅          | Cold storage via region-bound S3 buckets                 |
| KMS (Key Management Service)    | Regional  | ✅          | Region-based key management with multi-region support    |
| CloudFront (CDN)                | Global    | 🌐         | Global content delivery via edge locations               |
| API Gateway                     | Mixed     | 🔁         | Supports Regional, Edge-Optimized (global), or Private   |
| CloudWatch                      | Regional  | ✅          | Region-based logs, metrics, and dashboards               |
| CloudTrail                      | Mixed     | 🔁         | Regional logging; org-wide multi-region trails supported |
| EFS (Elastic File System)       | Regional  | ✅          | Region-scoped file systems, AZ-redundant                 |
| EBS (Elastic Block Store)       | Per AZ    | ✅          | Volumes tied to specific availability zones              |
| Lambda                          | Regional  | ✅          | Functions are deployed within regions                    |

---

### 🗝️ Emblem Legend:

* ✅ **Regional** – Service is deployed and used within a specific AWS region
* 🌐 **Global** – Service operates globally, not tied to any one region
* 🔁 **Mixed** – Supports both regional and global modes depending on configuration

Would you like this exported as a table image, PDF, or CSV file?
