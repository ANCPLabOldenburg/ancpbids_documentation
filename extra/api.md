# API
## *Class* BIDSLayout(ds_dir: str, **kwargs)
A convenience class to provide access to an in-memory representation of a BIDS dataset.
  
  dataset_path = 'path/to/your/dataset'
  layout = BIDSLayout(dataset_path)

**Parameters:**
* **ds_dir** – the (absolute) path to the dataset to load

### layout.get_<entity>()
It is possible to get a unique set of values for each of the available BIDS entities. See EntityEnum for supported entities.
Examples:

    all_task_labels = layout.get_tasks()
    all_subject_labels = layout.get_subjects(task='lang')
    all_bold_session_labels = layout.get_sessions(suffix='bold')

### layout.get(return_type: str = 'object', target: str | None = None, scope: str | None = None, extension: str | List[str] | None = None, suffix: str | List[str] | None = None, **entities) → List[str] | List[object]
Depending on the return_type value returns either paths to files that matched the filtering criteria or Artifact objects for further processing by the caller.

Note that all provided filter criteria are AND combined, i.e. subj=’02’,task=’lang’ will match files containing ‘02’ as a subject AND ‘lang’ as a task. If you provide a list of values for a criteria, they will be OR combined.
  
    file_paths = layout.get(subj='02', task='lang', suffix='bold', return_type='files')
    file_paths = layout.get(subj=['02', '03'], task='lang', return_type='files')
    
**Parameters:**
* **return_type** – Either ‘files’ to return paths of matched files or ‘object’ to return Artifact object, defaults to ‘object’
* **target** – Either suffixes, extensions or one of any valid BIDS entities key (see EntityEnum, defaults to None
* **scope** – a hint where to search for files If passed, only nodes/directories that match the specified scope will be searched. Possible values include: ‘all’ (default): search all available directories. ‘derivatives’: search all derivatives directories. ‘raw’: search only BIDS-Raw directories. ‘self’: search only the directly called BIDSLayout. <PipelineName>: the name of a BIDS-Derivatives pipeline.
* **extension** – criterion to match any files containing the provided extension only
* **suffix** – criterion to match any files containing the provided suffix only
* **entities** – a list of key-values to match the entities of interest, example: subj=’02’,task=’lang’
**Returns:**
* depending on the return_type value either paths to files that matched the filtering criteria
* or Artifact objects for further processing by the caller

### get_dataset() → object
**Returns:**
**Return type:** the in-memory representation of this layout/dataset.

### get_dataset_description(scope='self', all_=False) → List[Dict] | Dict
Return contents of dataset_description.json.

**Parameters:**
* **scope** *(str)* – The scope of the search space. Only descriptions of BIDSLayouts that match the specified scope will be returned. See bids.layout.BIDSLayout.get docstring for valid values. Defaults to ‘self’ –i.e., returns the dataset_description.json file for only the directly-called BIDSLayout.
* **all** *(bool)* – If True, returns a list containing descriptions for all matching layouts. If False (default), returns for only the first matching layout.
**Returns:** a dictionary or list of dictionaries (depending on all_).
**Return type:** dict or list of dict

#### get_entities(scope: str | None = None, sort: bool = False) → dict
Returns a unique set of entities found within the dataset as a dict. Each key of the resulting dict contains a list of values (with at least one element).

Example:
    entities = layout.get_entities()
    print(entities)
    # Output:
    # {
        # 'sub': ['01', '02', '03'],
        # 'task': ['gamblestask']
    # }
    
**Parameters:**
* **scope** – see BIDSLayout.get()
* **sort** (default is *False*) – whether to sort the keys by name
**Returns:** a unique set of entities found within the dataset as a dict
**Return type:** dict

### get_file(filename, scope='all')
Return the BIDSFile object with the specified path.

**Parameters:**
* **filename** *(str)* – The path of the file to retrieve. Must be either an absolute path, or relative to the root of this BIDSLayout.
* **scope** *(str or list, optional)* – Scope of the search space. If passed, only BIDSLayouts that match the specified scope will be searched. See BIDSLayout.get docstring for valid values. Default is ‘all’.
**Returns:** File found, or None if no match was found.
**Return type:** bids.layout.BIDSFile or None

### get_files(scope='all')
Get BIDSFiles for all layouts in the specified scope.

**Parameters:**
* **scope** *(str)* – The scope of the search space. Indicates which BIDSLayouts’ entities to extract. See bids.layout.BIDSLayout.get docstring for valid values.
**Returns:** A dict, where keys are file paths and values are bids.layout.BIDSFile instances.

### get_metadata(path, include_entities=False, scope='all')
Return metadata found in JSON sidecars for the specified file.

**Parameters:**
* **path** *(str)* – Path to the file to get metadata for.
* **include_entities** *(bool, optional)* – If True, all available entities extracted from the filename (rather than JSON sidecars) are included in the returned metadata dictionary.
* **scope** *(str or list, optional)* – The scope of the search space. Each element must be one of ‘all’, ‘raw’, ‘self’, ‘derivatives’, or a BIDS-Derivatives pipeline name. Defaults to searching all available datasets.
**Returns:** A dictionary of key/value pairs extracted from all of the target file’s associated JSON sidecars.
**Return type:** dict

**Notes:** A dictionary containing metadata extracted from all matching .json files is returned. In cases where the same key is found in multiple files, the values in files closer to the input filename will take precedence, per the inheritance rules in the BIDS specification.

### validate() → ValidationReport
Validates a dataset and returns a report object containing any detected validation errors.

Example

    report = layout.validate()
    for message in report.messages:
        print(message)
    if report.has_errors():
        raise "The dataset contains validation errors, cannot continue".
**Returns:**
* **Return type:** a report object containing any detected validation errors or warning

### write_derivative(derivative)
Writes the provided derivative folder to the dataset. Note that a ‘derivatives’ folder will be created if not present.

**Parameters:**
* **derivative** – the derivative folder to write

## class ancpbids.DatasetOptions(infer_artifact_datatype: bool = False, ignore: bool | List[str] = False)

All options that can be set to influence handling of reading/writing a dataset from/to file system.

### ignore: bool | List[str] = False
If a .bidsignore file is available at the root, all resources (files/folders) matching the filters in that file will not be added to the in-memory graph. Alternatively, a list of fnmatch patterns can be provided.

By default, this option is set to False as it may have a negative performance impact.

### infer_artifact_datatype: bool = False
If True, will determine the datatype an Artifact is contained in, either directly or within a sub-directory. For example, given the path “sub-02/func/sub-02_task-mixedgamblestask_run-01_bold.nii.gz”, the datatype will be “func”.

By default, this option is set to False as it may have a negative performance impact.

## ancpbids.load_dataset(base_dir: str, options: DatasetOptions | None = None)
Loads a dataset given its directory path on the file system.

    from ancpbids import load_dataset, validate_dataset
    dataset_path = 'path/to/your/dataset'
    dataset = load_dataset(dataset_path, DatasetOptions(ignore=False, infer_artifact_datatype=True))

**Parameters:**
* **base_dir** – the dataset path to load from
* **options** – the options to use, see DatasetOptions class for available options
**Returns:** a Dataset object depending on the used schema which represents the dataset as an in-memory graph
**Return type:** str

## ancpbids.load_schema(base_dir)
Loads a BIDS schema object which represents the static/formal definition of the BIDS specification.

As per BIDS spec, a BIDS compliant dataset must have a BIDSVersion field in the dataset_description.json file at the top level. This field is used to determine which BIDS schema to load.

In case the BIDSVersion field is missing or not supported, the earliest supported schema will be returned.

**Parameters:**
* **base_dir** – The dataset directory path where a dataset_description.json must be located.
**Returns:** A BIDS schema object which represents the static/formal definition of the BIDS specification.
**Return type:** object

## ancpbids.save_dataset(ds: object, target_dir: str, context_folder=None)
Copies the dataset graph into the provided target directory.

EXPERIMENTAL/UNSTABLE

**Parameters:**
* **ds** – the dataset graph to save
* **target_dir** – the target directory to save to
* **context_folder** – a folder node within the dataset graph to limit to

## ancpbids.validate_dataset(dataset) → ValidationReport
Validates a dataset and returns a report object containing any detected validation errors.

Example:

    report = validate_dataset(dataset)
    for message in report.messages:
        print(message)
    if report.has_errors():
        raise "The dataset contains validation errors, cannot continue".

**Parameters:**
* **dataset** – the dataset to validate
**Returns:** a report object containing any detected validation errors or warning
**Return type:** ValidationPlugin.ValidationReport

## ancpbids.write_derivative(ds, derivative)
Writes the provided derivative folder to the dataset. Note that a ‘derivatives’ folder will be created if not present.

**Parameters:**
* **ds** – the dataset object to extend
* **derivative** – the derivative folder to write

## ancpbids.utils.deepupdate(target, src)
This function recursively merges two dictionaries — src into target — in a smart way that handles nested dictionaries, lists, and sets. It modifies target in-place, adding or updating keys from src.
For each key–value pair in src:
* If the key does not exist in target, the value is "deep" copied in.
* If the value is a list, it's appended to the list in target.
* If the value is a set, it's update into the set in target.
* If the value is a dictionary, it's recursively merged with the existing dict.

Examples:
      t = {‘name’: ‘Ferry’, ‘hobbies’: [‘programming’, ‘sci-fi’]}
      deepupdate(t, {‘hobbies’: [‘gaming’]})
      print(t)
      # Output
      # {‘name’: ‘Ferry’, ‘hobbies’: [‘programming’, ‘sci-fi’, ‘gaming’]}

Copyright Ferry Boender, released under the MIT license.

## ancpbids.utils.fetch_dataset(dataset_id: str, output_dir='~/.ancp-bids/datasets')
Downloads and extracts an ancpBIDS test dataset from Github.

**Parameters:**
* **dataset_id** – The dataset ID of the ancp-bids-datasets github repository. See https://github.com/ANCPLabOldenburg/ancp-bids-dataset for more details.
* **output_dir** – The output directory to download and extract the dataset to. Default is to write to user’s home directory at ~/.ancp-bids/datasets

**Returns:** The path of the extracted dataset.
**Return type:** str






