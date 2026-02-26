# 2026
##  query_entities (line 131)

    subjects: List[str] = sorted(list(dataset.query_entities(scope=SCOPE).get("subject", [])))
    print(subjects)
    # ['009']

## Query raw MEG artifacts for this subject (line 147)

```bash

SCOPE = "raw"
SUFFIX = "meg"
EXTENSIONS = [".fif", ".fif.gz"]


raw_artifacts = list(dataset.query(
    subj=sid,
    suffix=SUFFIX,
    scope=SCOPE,
    extension=EXTENSIONS,
    return_type="object",
))

print(raw_artifacts)

#Output:
# [{'name': 'sub-009_ses-1_task-deduction_run[...]', 'extension': '.fif', 'suffix': 'meg'}, {'name': 'sub-009_ses-1_task-induction_run[...]', 'extension': '.fif', 'suffix': 'meg'}]


```
