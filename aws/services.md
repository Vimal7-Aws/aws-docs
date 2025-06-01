| **Service**                                  | **Regional / Global**                 | **Notes**                                                                       |
| -------------------------------------------- | ------------------------------------- | ------------------------------------------------------------------------------- |
| **ALB (Elastic Load Balancing - ALB)**       | ✅ Regional                            | Must be launched in a specific VPC in a region                                  |
| **Route 53**                                 | 🌐 Global                             | Global service but interacts with regional resources (e.g. health checks)       |
| **IAM**                                      | 🌐 Global                             | Roles, policies, and users are global across all regions in the account         |
| **S3**                                       | ✅ Regional (buckets are region-bound) | Bucket is created in a specific region, but service is considered global        |
| **Kinesis**                                  | ✅ Regional                            | Streams, Firehose, and Analytics are region-specific                            |
| **ECS (Elastic Container Service)**          | ✅ Regional                            | Services, tasks, and clusters are region-scoped                                 |
| **DynamoDB**                                 | ✅ Regional                            | Tables live in a single region unless using Global Tables                       |
| **RDS**                                      | ✅ Regional                            | Instances live in specific regions and AZs                                      |
| **Aurora**                                   | ✅ Regional                            | Amazon Aurora is part of RDS, scoped to region and AZs                          |
| **Subnets**                                  | ✅ Regional (per AZ)                   | Subnets are created in specific availability zones (AZs) within a region        |
| **VPC**                                      | ✅ Regional                            | VPC is regional; subnets inside VPC are per AZ                                  |
| **NACL (Network ACLs)**                      | ✅ Regional (per VPC)                  | Associated with subnets inside regional VPCs                                    |
| **Glacier (now part of S3 Storage Classes)** | ✅ Regional                            | Glacier Deep Archive and Instant Retrieval are S3 storage classes, region-bound |
| **KMS**                                      | ✅ Regional                            | Keys are created and used within a specific region                              |
