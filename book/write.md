# Create a Derivative
An important functionality of ancpBIDS is to create and write derivatives.

## Create a Derivative in the Memory Graph
The function `create_derivative` creates within the [in-memory graph](https://ancplaboldenburg.github.io/ancpbids_documentation/extra/inmemory.html), nothing is written to disk yet. You can also add your desired name (e.g., 'Meg_QC') as well as a description to the derivatives folder.This tep only defines what will be written later.

```bash
  derivative = dataset.create_derivative(name= "Meg_QC")
  derivative.dataset_description.GeneratedBy.Name = "MEG header extraction"
```

### Create a Folder
You can also create deeper levels of folders, for example, for specific pipelines within an analysis with the function `create_folder`.

```bash
    calculation_folder = derivative.create_folder(name="calculation")
```

You can also create specific subject folder using the function `create_folder` along with specifying the `schema`.

```bash
subject_folder = calculation_folder.create_folder(
            type_=schema.Subject,
            name="sub-" + sid
        )
```








## very old

`write_derivative()` saves the provided derivative folder to the dataset. Note that a ‘derivatives’ folder will be created if not present. Optionally, you may also use the `DatasetOptions` class to set your preference in the handling of writing a derivative to your file system.




```bash
from ancpbids import write_derivative

target_dir = '/path/to/your/target/directory'
write_derivative(dataset, target_dir)
```



```{important}

TODO: provide a more elaborative example using write_derivative() (deprecated) and artifact.write() (preferred)

```

-->

<!--
# Saving a Dataset

Once you've queried and worked with your BIDS dataset, you may want to save your dataset to your local file system. This can be done with several functions. 
the ´load_dataset()´ function. Optionally, you may also  also use the ´DatasetOptions´ class to set your preference in the handling of reading (and writting) a dataset from your file system. In the following example we will use the ´dataset_path´ you have queried beforehand.

## Saving a Dataset
Once your work is finished with your dataset, you can save it back to disk using ´save_dataset()´. Your ´target directory´ should be empty.


    from ancpbids import save_dataset

    target_dir = '/path/to/your/traget/directory'
    save_dataset(dataset, target_dir)

    #ValueError: No file writer registered for file: /.../sub-009_ses-1_scans.tsv


When calling `save_dataset()`, the [in-memory graph](https://ancplaboldenburg.github.io/ancpbids_documentation/extra/inmemory.html) materializes as a new folder on disk. This is done "schema-aware": following the syntax and the semantic BIDS specification, such as naming Artifacts with correct key-value pairs. The new folder will contain the [dataset_description.json](https://alexisbaxman.github.io/ancpbids_documentation/extra/inmemory.html#the-model-of-a-bids-dataset), with the field `BIDSVersion` derived directly from the schema.

![dataset_description_json](../static/dataset_description_json.png)

```{note}
For each subject, a separate folder is created, and **Artifacts are named automatically:** the key-value information (such as *'sub-09'*) is inferred from the folder structure. This writting functionally allows to export fully valid BIDS derivatives dynamically and automatic within pipelines. 

```
-->



