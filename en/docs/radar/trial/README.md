# Trial

This directory holds radar entries in trial: real use in a limited, named scope, such as one project, one machine, one client, or a fixed period. Assess asks what a product does; trial asks how it lives with us.

Trial is optional. It is for questions testing cannot answer, such as what something costs to run over time, whether it stays accessible in daily use, or whether a process holds up in real work. Testing itself happens in assess.

## Entering trial

An entry comes here from assess only when every gate criterion and the Security section carries a Verified tag, or a named person has recorded an override. Its status notes state the scope of the trial and the end condition before the trial starts.

## Leaving trial

When the end condition is reached, a human records an evaluation pass in the entry's `uc-radar_evaluation` block and moves the entry to adopt, hold, or rejected, stating why in the status notes.

See the [UC Radar Evaluation Lifecycle](../uc-radar--evaluation-lifecycle-v0.2.0.md) for the full definitions.
