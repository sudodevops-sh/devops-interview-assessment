# Technical Assessment Part 2: Infrastructure as Code (CloudFormation)

## Objective

Architect a resilient, serverless event-processing pipeline using AWS CloudFormation. You will provision the database, the compute layer, and the secure integration between them.

## Scenario

You are building the infrastructure for the **"Loyalty Rewards"** system. The goal is to ensure that the DynamoDB Table, the Lambda function (from Part 1), and the necessary permissions are deployed as a single, reproducible stack.

## Requirements

### 1. Database Layer (DynamoDB)

- **Table Name**: `OrdersTable`
- **Schema**: Partition Key `OrderId` (String).
- **Capacity**: Use `PAY_PER_REQUEST` billing mode.
- **Streams**: Enable DynamoDB Streams.

### 2. Compute Layer (Lambda)
- **Runtime**: `python3.12` (or latest).
- **Code Injection**: For this assessment, you may embed your Python code from Part 1 directly into the template using the ZipFile property.
- **Configuration**: Pass the Table Name to the function via an Environment Variable.

### 3. Security & Permissions (IAM)
- **Role**: Create a dedicated IAM Execution Role for the Lambda.
- **Principle of Least Privilege**: Do not use `AdministratorAccess` or `*` on all resources.
    - Grant `UpdateItem` only to the specific Table ARN.
    - Grant Stream reading permissions only to the specific Stream ARN.
    - Include permissions for CloudWatch Logs.

### 4. Integration (Event Source Mapping)
- Trigger: Map the DynamoDB Stream to the Lambda function.



## Technical Constraints

- The entire solution must be contained within a **single CloudFormation template** (`template.yaml` or `template.json`).


## Deliverables
- `template.yaml`: The complete CloudFormation template.