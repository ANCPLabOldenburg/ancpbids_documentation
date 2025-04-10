# Advanced Queries

## Retrieving matching filenames

The layout.get() function allows for more complex queries and can return a **list of files** matching your **criteria** according to your parameters.


````{tab-set}
```{tab-item} MEG

    file_paths = layout.get(suffix='meg', subject='009', return_type='filename')
    print("MEG files of subject 009:", *file_paths, sep='\n')

    #Output:
    #MEG files of subject 009:

```

```{tab-item} MRI

    file_paths = layout.get(suffix='bold', subject='02', return_type='filename')
    print("BOLD files of subject 2:", *file_paths, sep='\n')

    #Output:
    #BOLD files of subject 2:
    #/Users/*yourUserName*/.ancp-bids/datasets/ds005/sub-02/func/sub-02_task-mixedgamblestask_run-01_bold.nii.gz
    #/Users/*yourUserName*/.ancp-bids/datasets/ds005/sub-02/func/sub-02_task-mixedgamblestask_run-02_bold.nii.gz
    #/Users/*yourUserName*/.ancp-bids/datasets/ds005/sub-02/func/sub-02_task-mixedgamblestask_run-03_bold.nii.gz

```
````

the get() function can simultaneously search for matches with the following parameters:

```{admonition} Parameters
:class: dropdown

* **Scope:** The BIDS subdirectory to be searched. Can be ‘raw’ or ‘derivatives’.
* **Entities:** Key-value pairs in the filenames are entities defined in BIDS. Examples are ‘sub’ or ‘run’. Use layout.get_entities() to get a list of entities available in the dataset.
* **Suffix:** Suffixes define the imaging modality or data type. Examples are ‘bold’ or ‘meg’ but also ‘events’ or ‘participants’.
* **Extension:** Is the file extensions. Examples are ‘.nii’ or ‘nii.gz’ for MRI, ‘.fif’ for MEG or '.tsv' for tabular files.
* **Return_type:**Defines the what get() returns. This can be ‘filename’ or ‘dict’, where ‘dict’ is the default.

```


We can use these parameters to **narrow down** or **broaden** our queries. For example, if we want to query for the json metadata file (extension) which contain information about the rawdata (scope) we can use layout.get() with the appropriate parameters:

````{tab-set}
```{tab-item} MEG

    meg_timeseries_files = layout.get(scope='raw', return_type='filename', suffix='meg', extension='.json', sub='009', task=  ['induction','deduction'])
    print(*meg_timeseries, sep='\n')

    #Output:
    #./
    #/Users/*yourUserName*/.ancp-bids/datasets/ds003483/sub-009/ses-1/meg/sub-009_ses-1_task-deduction_run-1_meg.json
    #/Users/*yourUserName*/.ancp-bids/datasets/ds003483/sub-009/ses-1/meg/sub-009_ses-1_task-induction_run-1_meg.json

```

```{tab-item} MRI

    bold_files = layout.get(scope='raw',
                    return_type='filename',
                    suffix='bold',
                    extension='.json',
                    sub='03',
                    task='mixedgamblestask',
                    run=["01", "02"])
    print(*bold_files, sep='\n')
    #Output:
    #/Users/*yourUserName*/.ancp-bids/datasets/ds005/sub-03/func/sub-03_task-mixedgamblestask_run-01_bold.json
    #/Users/*yourUserName*/.ancp-bids/datasets/ds005/sub-03/func/sub-03_task-mixedgamblestask_run-02_bold.json

```
````

The parameters that you can manipulate strongly depend on the dataset. Every entity defined in your data can be a parameter of the get() function.

Now we can also **not** specify certain parameters in our query to **broaden** our query. For example, if we don’t specify the 'sub' parameter, we will receive a list containing all the specified filepaths for all subjects, not only subject 009.


````{tab-set}
```{tab-item} MEG

    meg_timeseries_fif_files = layout.get(scope='raw', return_type='filename', suffix='meg', extension='.fif', task=['induction','deduction'])
    print(*meg_timeseries_fif_files, sep='\n')

    #Output:
    #All the .fif files of every subject, for both tasks. It's a very long output.

```

```{tab-item} MRI

    bold_niigz_files = layout.get(scope='raw',return_type='filename',suffix='bold',extension='.nii.gz', task='mixedgamblestask', run0["01","02"])
    print('bold nii.gz files: ')
    print(*bold_niigz_files, sep='\n')

    #Output:
    #All bold files for all subjects for both runs of the task

```
````

## Next Section
In the next section we will cover how to retrieve metadata from JSON sidecar files, access BIDS-specific suffix files and filtering entity key.
    

