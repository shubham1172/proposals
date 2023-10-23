# Title of proposal 

* Author(s): Shubham Sharma (@shubham1172), Deepanshu Agarwal (@DeepanshuA)
* State: Ready for Implementation
* Updated: 10/23/2023

## Overview

Dapr supports multiple SDKs and each SDK reports errors for different scenarios in its own way with inconsistent messaging.

This makes troubleshooting and debugging difficult. Moreover, when provided with logs from different SDKs, it is often hard to compare the errors and understand if they are related or not.

This is an extension of the [Dapr Error Handling/Codes](./0009-BCIRS-error-handling-codes.md) proposal, which proposes error code standardization for Dapr runtime and CLI. This proposal extends the same to Dapr SDKs.

## Background

The need for this proposal arises from the inconsistency in error handling across different SDKs. Currently, error codes and messages are separate for each SDK, resulting in a lack of standardization. This makes it difficult to troubleshoot issues and debug them.

Taking the example of [Dapr Workflow](https://docs.dapr.io/developing-applications/building-blocks/workflow/workflow-overview/), 
TODO explain.

## Related Items

### Related proposals 

- [Dapr Error Handling/Codes](./0009-BCIRS-error-handling-codes.md)

### Related issues

## Expectations and alternatives

### What is in scope for this proposal?
1. Creating a new repository, `dapr/sdk-core`, which will contain the error codes and messages for all the SDKs.
1. Defining and documenting error codes for Dapr Workflow feature.
1. Developing code-gen tool to generate libraries for different languages.
1. Writing GitHub workflows to publish the generated libraries.
1. Consuming the generated libraries in the core SDKs - Go, Java, Python, Dotnet, and JavaScript (only if they support Dapr Workflow).

### What is deliberately *not* in scope?
1. Error codes or messages for other Dapr features. This can be added later.
1. Adding workflow support to SDKs that do not support it currently.

### What alternatives have been considered, and why do they not solve the problem?

The other alternative was a strategy that closely resembles the Azure SDKs.
1. Documenting the error codes as a reference for users and SDKs. Example: [Table Storage error codes (REST API) - Azure Storage | Microsoft Learn](https://learn.microsoft.com/en-us/rest/api/storageservices/table-service-error-codes)
1. Coding the error code in SDKs. Example: [aztables/error_codes.go](https://github.com/Azure/azure-sdk-for-go/blob/f951bf52fb68cbb978b7b95d41147693c1863366/sdk/data/aztables/error_codes.go) and [azure/data/tables/_error.py](https://github.com/Azure/azure-sdk-for-python/blob/def1f2b7f1338eb1bd732a57127945921f3f1092/sdk/tables/azure-data-tables/azure/data/tables/_error.py#L273)
1. Writing automated tests that can compare parity between the documentation and the SDKs. Each SDK has its own test that fails in case there is a mismatch.

This, however, has the following drawbacks:
1. Requires initial work to add the changes manually to each SDK.
1. Requires setting up pipelines in each SDK that reference the documentation and run the tests.
1. It is not scalable. With each addition to common codes or logic, it will end up requiring work in each SDK, as well as the tests.

### What advantages / disadvantages does this proposal have? 

#### Advantages

1. Initial work requires only setting up the common data and writing code-gen
1. It scales with easy onboarding of new features and SDKs.

#### Disadvantages

1. Requires maintaining a separate repository for the common data.
1. With each release, the SDKs will have to be updated to the latest version of the common data.

## Implementation Details

### Design

How will this work, technically? Where applicable, include: 

* Design documents
* System diagrams
* Code examples

### Feature lifecycle outline

* Expectations
* Compatability guarantees
* Deprecation / co-existence with existing functionality
* Feature flags

### Acceptance Criteria

How will success be measured? 

* Performance targets
* Compabitility requirements
* Metrics

## Completion Checklist

What changes or actions are required to make this proposal complete? Some examples:

* Code changes
* Tests added (e2e, unit)
* SDK changes (if needed)
* Documentation

