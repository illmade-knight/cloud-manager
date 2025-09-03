# **Prompt 05: Implementation for BigQueryIAMManager (Admin Tool)**

**Objective:** Generate the complete implementation for the `BigQueryIAMManager`. The generated code must satisfy the previously defined unit and integration tests.

**TDD Phase:** Green

## **Primary Prompt (Concise)**

Your task is to generate the Go code for the `iam/google_dataset_iam.go` file.

Your implementation must correctly manage BigQuery dataset permissions by translating modern IAM roles into legacy ACL entries and using the "get-modify-set" pattern on the dataset's metadata.

## **Reinforcement Prompt (Detailed Context)**

**Your Specific Task:** Generate the complete code for `iam/google_dataset_iam.go`.

**Key Requirements to Fulfill:**

* **Pass All Tests.**
* **Role Translation:** You must implement a `translateToLegacyRole` helper function that correctly maps modern IAM roles (`roles/bigquery.dataViewer`, `roles/bigquery.dataEditor`) to their legacy `bigquery.AccessRole` equivalents (`ReaderRole`, `WriterRole`).
* **(L2-2.2) "Get-Modify-Set" Pattern:** The `AddDatasetIAMBinding` method **must** be implemented using the safe get-modify-set pattern on the dataset's `Metadata.Access` slice.
* **Translation-Aware Logic:**
    * The `AddDatasetIAMBinding` method should add a new `bigquery.AccessEntry` with `EntityType: bigquery.IAMMemberEntity`.
    * The `CheckDatasetIAMBinding` and `RemoveDatasetIAMBinding` methods are more complex. They **must** first translate the modern role to its legacy equivalent and then search the `Access` list for the corresponding legacy entry (e.g., `EntityType: UserEmailEntity` with a bare email `Entity`). This is required to correctly interact with the real API behavior.

**Dependencies (Full source code to be provided):**

1.  **The Specifications:** `google_dataset_iam_test.go` and `google_dataset_iam_integration_test.go`

---

## Prompt Changelog

### RV_20250901

This is the initial version of the prompt for the `BigQueryIAMManager` implementation. It requires the complex logic needed to handle the translation between modern IAM roles and BigQuery's legacy dataset ACLs.