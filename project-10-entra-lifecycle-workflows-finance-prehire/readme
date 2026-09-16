# Troubleshooting

## Incident 2 – Finance Joiner Did Not Appear in Workflow Scope

The workflow was configured with:

`department eq 'Finance'`

However, the test Joiner account contained:

`Department = Finances`

When the workflow was evaluated using **What If**, no users appeared
under **Users in Scope**.

### Investigation

The following items were verified:

- Workflow enabled
- Schedule enabled
- Time-based trigger configured
- employeeHireDate populated
- Department scope configured

The identity attribute was then compared with the workflow scope.

Workflow expected:

`Finance`

User attribute contained:

`Finances`

### Root Cause

The user did not satisfy the workflow scope because the Department
attribute did not match the configured scope value.

### Resolution

The Department value was corrected:

`Finances → Finance`

The workflow was then tested again using What If.

### Validation

After correcting the identity attribute and satisfying the time-based
trigger condition, Anjali Rao appeared under **Users in Scope**.
