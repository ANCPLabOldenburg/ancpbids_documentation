# Loading and Writting a Dataset

## Loading a Dataset

Once you've queried and worked with your BIDS dataset, you may want to load your dataset from your local file system. This is done with the ´load_dataset()´ function. Optionally, you may also  also use the ´DatasetOptions´ class to set your preference in the handling of reading (and writting) a dataset from your file system. In the following example we will use the ´dataset_path´ you have queried beforehand.

```{tab-item} Simple loading

  from ancpbids import load_dataset
  dataset = load_dataset(dataset_path)
  # print(dataset)
  # {'name': 'ds003483'}

```

```{tab-item} Loading with DatasetOptions

  from ancpbids import load_dataset, DatasetOptins
  dataset = load_dataset(dataset_path, DatasetOptions(ignore=False, infer_artifact_datatype=True))
  # print(dataset)
  # {'name': 'ds003483'}

```


```{admonition} DatasetOptions:
:class: dropdown
It allows you to customize how the dataset is interpreted from the file system.
* ´ignore:´If set to True, ancpBIDS will respect the .bidsignore file (if present) at the root of the dataset. Default if ´False´ as it may have a negative performance impact.

* ´infer_artifact_datatype:´ If True, ancpBIDS will try to infer the datatype of each Artifact (like func, anat, meg) based on the file path. By default, this option is set to False as it may have a negative performance impact.

```






save processed data or generate derivative datasets. Unlike PyBIDS, ´ancpBIDS´ supports **writing new datasets directly from its [in-memory graph](https://ancplaboldenburg.github.io/ancpbids_documentation/extra/inmemory.html).**

    ancpbids.save_dataset(ds: object, target_dir: str, context_folder=None)


When calling `save_dataset()`, the in-memory graph materializes as a new folder on disk. This is done "schema-aware": following the syntax and the semantic BIDS specification, such as naming Artifacts with correct key-value pairs. The new folder will contain the [dataset_description.json](https://alexisbaxman.github.io/ancpbids_documentation/extra/inmemory.html#the-model-of-a-bids-dataset), with the field `BIDSVersion` derived directly from the schema.

![dataset_description_json](../static/dataset_description_json.png)

```{note}
For each subject, a separate folder is created, and **Artifacts are named automatically:** the key-value information (such as *'sub-09'*) is inferred from the folder structure. This writting functionally allows to export fully valid BIDS derivatives dynamically and automatic within pipelines. 

```




