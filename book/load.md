# Load a BIDS Dataset
Before you can query or modify your data, you need to make your dataset available to ancpBIDS. This involves two steps: **fetching a dataset** and **creating a layout** to represent its structure.

## 1. Fetch an existing BIDS dataset
ancpBIDS is able to load specific files from your PC. With ´fetch_dataset()´, ancpBIDS will be able to load it even if you haven't unzip your dataset. 

````{tab-set}
```{tab-item} MEG

    ```bash
    from ancpbids import utils
    dataset_path = utils.fetch_dataset('ds005')
    ```


```

```{tab-item} MRI

    ```bash
    from ancpbids import utils
    dataset_path = utils.fetch_dataset('ds003483')
    ```

```
````

The output variable, ´dataset_path´, will contain the local path to your dataset.

Also, within your PC folder

```bash
home/user/.ancp-bids/datasets
```

you will find the zip file (f.e. ds005-testdata.zip) and the content extracted (f.e. ds005). If you run the code again, it won't create unnecessary copies.


```{admonition} Don't have a Dataset?
:class: tip
In case you don't have a BIDS compliant dataset, you can download a test dataset from our [github](https://github.com/ANCPLabOldenburg/ancp-bids-dataset). We offer you two types: a **MEG** dataset (`ds003483`) and a **MRI** dataset (`ds005`). If you have your own BIDS dataset, feel free to use yours instead.
These datasets are only meant to learn how to use ancpBIDS, and are not expected to be used in any kind of research. 

```

## 2. Creating a Layout
Once you have your dataset available locally on your PC, we can use ´BIDSLayout()´ to create the in-memory graph of it, from where you can easily retrieve information. Both the dataset and the schema are accessible through the layout object.

```bash
from ancpbids.pybids_compat import BIDSLayout
layout = BIDSLayout(dataset_path)
```
  
```{admonition} In-memory graph?
:class: tip

If you want to learn more how ancpBIDS uses the BIDS specification to build the in-memory graph representation (and what exactly is a in-memory graph representation), [follow this link](https://alexisbaxman.github.io/ancpbids_documentation/extra/inmemory.html).

```
    
### Load a dataset
**Thanks to BIDSLayout your dataset it's already loaded within the layout variable.**

Alternatively, you may also use the function ´load_dataset()´ along with the dataset_path you fetched before to load the dataset. 


````{tab-set}
```{tab-item} Simple loading

    ```bash
    from ancpbids import load_dataset
    dataset = load_dataset(dataset_path)
    #print(dataset)
    #{'name': 'ds003483'}
    ```

```

```{tab-item} Loading with DatasetOptions

    ```bash
    from ancpbids import load_dataset, DatasetOptins
    dataset = load_dataset(dataset_path, DatasetOptions(ignore=False, infer_artifact_datatype=True))
    #print(dataset)
    #{'name': 'ds003483'}
    ```

```
````



## Next section
Now that your dataset is loaded and mapped into a layout, you're ready to start extracting useful information from it. In the next sections, we'll show how to:
* [Basic Queries](https://ancplaboldenburg.github.io/ancpbids_documentation/query/basic.html) using common entities, like subjects and tasks.
* [Adanced Queries](https://ancplaboldenburg.github.io/ancpbids_documentation/query/advanced.html) using specific files using parameters.
* [Retrieve metadata](https://ancplaboldenburg.github.io/ancpbids_documentation/query/metadata.html) from sidecars and filenames.

