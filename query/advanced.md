# Advanced Queries 

## Retrieving matching filenames

The `get()` function allows for more complex queries, ancpBIDS allow a series of parameters that you can manipulate, apart from common `entities` (subject, run, task...).


```{admonition} ancpBIDS query parameters
:class: tip

This parameters are not BIDS entities, they are just used to control search location.

* **scope:**  Defines which subdirectory to be searched, commmon values are `'raw'` or `'derivatives'`.  
* **suffix:** Refers to the BIDS filename suffix, which indicates the modality or data type. Examples are `‘bold’` or `‘meg’`, but there are also specific suffix for metadata, such as `'events'` (see below).
* **extension:** Specifies the file extension or format. Examples include `‘.nii’` or `‘nii.gz’` for MRI, `‘.fif’` for MEG or `‘.tsv’` for tabular files.
* **return_type:** Determines the type of object returned by the query. It may return filenames or dictionaries (‘dict’ is the default).

```

A simple query consist on specifying the entity participant (`subject`) and the imaging modality (`suffix`):

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


We can use these parameters to **narrow down** or **broaden** our queries. For example, if we want to query for a sidecar (a JSON file with metadata) which contains information about the rawdata of participant 9 we can use `layout.get()` with the appropriate parameters: 

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
    # /Users/*yourUserName*/.ancp-bids/datasets/ds005/sub-03/func/sub-03_task-mixedgamblestask_run-01_bold.json
    # /Users/*yourUserName*/.ancp-bids/datasets/ds005/sub-03/func/sub-03_task-mixedgamblestask_run-02_bold.json

```
````


Now we can also **not** specify certain parameters in our query to **broaden** our query. For example, if we don’t specify the `sub` parameter, we will receive a list containing all the specified filepaths for all subjects, not only subject 009.


````{tab-set}
```{tab-item} MEG

    meg_timeseries_fif_files = layout.get(scope='raw', return_type='filename', suffix='meg', extension='.fif', task=['induction','deduction'])
    print(*meg_timeseries_fif_files, sep='\n')

    #Output:
    # All the .fif files of every subject, for both tasks. It's a very long output.

```

```{tab-item} MRI

    bold_niigz_files = layout.get(scope='raw', return_type='filename',suffix='bold',extension='.nii.gz', task='mixedgamblestask', run0["01","02"])
    print('bold nii.gz files: ')
    print(*bold_niigz_files, sep='\n')

    #Output:
    # All bold files for all subjects for both runs of the task

```
````


## Querying for Metadata Files

The `suffix` parameter can also be used to find metadata files, which contain valuable information about your events, channels, or scanning sessions.

```{admonition} Common suffixes in MEG data:

* **events:** search for event files (e.g., events.tsv), which contains time_stamps and event markers.
* **coordystem:** search for the file specifying the coordinate system used in the recording.
* **channels:** search for the file which specifies channel names and types (e.g. channels.tsv).
* **scans:** search for the files documenting the different scan sequences that were run (e.g., scans.tsv).

```

Here are some examples of how to query for your **event files** with the suffix `events`:


````{tab-set}
```{tab-item} MEG

    all_events = layout.get(suffix='events',return_type='filename')
    print(all_events)

    #Output
    #['./ancp-bids/tests/data/ds003483/sub-009/ses-1/meg/sub-009_ses-1_task-deduction_run-1_events.tsv',
    #'./ancp-bids/tests/data/ds003483/sub-009/ses-1/meg/sub-009_ses-1_task-induction_run-1_events.tsv',
    #...
    #'./ancp-bids/tests/data/ds003483/sub-031/ses-1/meg/sub-031_ses-1_task-deduction_run-1_events.tsv',
    #'./ancp-bids/tests/data/ds003483/sub-031/ses-1/meg/sub-031_ses-1_task-induction_run-1_events.tsv']

```

```{tab-item} MRI

    all_events = layout.get(suffix='events', return_type='filename')
    print(all_events)

    #Output: It will give you all the events, both .tsv and .json files for every participant. It's a very long output.

```
````

The `get()` function allows you to combine multiple parameters to narrow down your search based on your specific needs.  For example, you can specify if you want to search for your event files in `.json` or `.tsv` format with the parameter `extension`.

````{tab-set}
```{tab-item} MEG

    events_sub009_deduc = layout.get(suffix='events', subject='009', extension='.tsv', task='deduction', returntype='filename')
    print(events_sub009_deduc)

    #Output
    #The tsv file of the deduction task of the ninth subject: 
    #['./ancp-bids/test/data/ds003483/sub-009/ses-1/meg/sub-009_ses-1_task-deduction_run-1_events.tsv']

```
```{tab-item} MRI

    events_sub08 = layout.get(suffix='events', sub='08', run='02', extension='.tsv', return_type='filename')
    print(events_sub08)

    #Output
    #The tsv file of the 2nd run of the eigth subject: 
    #['/home/abaxman/.ancp-bids/datasets/ds005/sub-08/func/sub-08_task-mixedgamblestask_run-02_events.tsv']

```
````


## Extracting Metadata from Sidecars
Metadata stored in JSON files can be queried using `layout.get_metadata` along the directory path. This will return a dictionary with keys and values.


````{tab-set}
```{tab-item} MEG

    metadata = layout.get_metadata(layout.get(task='induction', suffix='meg',return_type='filename')[3])

    print("metadata: ", list(metadata.keys()))
    print("Value of Dewar position: ", metadata['DewarPosition'])

    #Output:
    #metadata:  ['AssociatedEmptyRoom', 'CapManufacturer',
    #'CapManufacturersModelName', 'ContinuousHeadLocalization',
    #'DeviceSerialNumber', 'DewarPosition', 'DigitizedHeadPoints',
    #'DigitizedLandmarks', 'ECGChannelCount', 'ECOGChannelCount',
    #'EEGChannelCount', 'EEGPlacementScheme', 'EEGReference',
    #'EMGChannelCount', 'EOGChannelCount', 'EpochLength',
    #'HeadCoilFrequency', 'InstitutionAddress', 'InstitutionName',
    #'InstitutionalDepartmentName', 'Instructions', 'MEGChannelCount',
    #'MEGREFChannelCount', 'Manufacturer', 'ManufacturersModelName',
    #'MiscChannelCount', 'PowerLineFrequency', 'RecordingDuration',
    #'RecordingType', 'SEEGChannelCount', 'SamplingFrequency',
    #'SoftwareFilters', 'SoftwareVersions', 'SubjectArtefactDescription',
    #'TaskDescription', 'TaskName', 'TriggerChannelCount', 'Description',
    #'RawSources', 'Authors', 'BaselineCorrection', 'BaselineCorrectionMethod',
    #'BaselinePeriod']
    #Value of Dewar position: 'upright'

```
```{tab-item} MRI

    metadata = layout.get_metadata(layout.get(task='mixedgamblestask', suffix='bold',return_type='filename')[3])

    print("metadata: ", list(metadata.keys()))
    print("Value of RepetitionTime: ", metadata['RepetitionTime'])

    #Output:
    #metadata:  ['RepetitionTime', 'TaskName', 'SliceTiming']
    #Value of RepetitionTime:  2.0

```
````
