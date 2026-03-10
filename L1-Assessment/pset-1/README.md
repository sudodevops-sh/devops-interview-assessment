# Technical Assessment Part 1: Event-Driven Stream Processor (Python/Boto3)

## Objective

Develop a  AWS Lambda function in Python using the boto3 SDK that processes real-time data changes from a DynamoDB Stream.

## Scenario

Our e-commerce platform tracks customer orders in a DynamoDB table. We need to implement a **"Loyalty Rewards"** feature. When an order's status is updated to **`DELIVERED`**, the system must automatically calculate reward points and update the order record with those points.

## Requirements

### 1. Functional Requirements
- Trigger: The function will be triggered by a DynamoDB Stream (configured in Part 2).
- Filtering: Only process records where the eventName is MODIFY.
- Logic: Compare the OldImage and NewImage.
    - If the Status changed from something else to **`DELIVERED`**, calculate points.
    - Points Formula: ``` RewardPoints = OrderTotal × 0.10 ``` (Round to the nearest integer).
- Data Persistence: Update the same DynamoDB item with a new attribute: RewardPoints.

### 2. Technical Guardrails 
- Infinite Loop Prevention: Since you are updating the same table that triggers the stream, you must implement logic to ensure the Lambda does not trigger itself indefinitely.

- Idempotency: Ensure that re-running the same stream event does not result in duplicate processing or inconsistent data.

- Environment Agnostic: Use an environment variable named **`TABLE_NAME`** for all Boto3 resource calls.

- Resiliency: Implement botocore.exceptions handling for API throttling or conditional check failures.

## Expected Data Structure

The Lambda function will receive a **standard DynamoDB Stream event**.

Assume the `NewImage` contains the following attributes:

| Attribute       | Type   | Description             |
| --------------- | ------ | ----------------------- |
| `OrderId`       | String | Unique order identifier |
| `Status`        | String | Current order status    |
| `OrderTotal`    | Number | Total order amount      |
| `CustomerEmail` | String | Customer email address  |

---

## Deliverables
- lambda_function.py: The complete Python source code.

- Short Write-up: A few sentences explaining your strategy for loop prevention and how you handled partial batch failures. (Optional)