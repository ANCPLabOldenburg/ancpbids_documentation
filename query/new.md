# 2026
##  query_entities: 131
subjects: List[str] = sorted(list(dataset.query_entities(scope=SCOPE).get("subject", [])))
    print(subjects)
['009']

## query raw meg artifact: 147

        # Query raw MEG artifacts for this subject
        raw_artifacts = list(dataset.query(
            subj=sid,
            suffix=SUFFIX,
            scope=SCOPE,
            extension=EXTENSIONS,
            return_type="object",
        ))

        print(raw_artifacts)
[{'name': 'sub-009_ses-1_task-deduction_run[...]', 'extension': '.fif', 'suffix': 'meg'}, {'name': 'sub-009_ses-1_task-induction_run[...]', 'extension': '.fif', 'suffix': 'meg'}]


## Next Section
