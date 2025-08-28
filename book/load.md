# Load a BIDS Dataset

Before you can query or modify your data, you need to "load" your dataset available to ancpBIDS.

```{admonition} Don't have a Dataset?
:class: dropdown

In case you don't have a BIDS compliant dataset, you can download a test dataset from our [github](https://github.com/ANCPLabOldenburg/ancp-bids-dataset) using `fetch_dataset()`. 

    from ancpbids import utils
    dataset_path = utils.fetch_dataset('ds005')

The output variable, `dataset_path`, will contain the local path to your dataset.

We offer an MEG dataset (`ds005`) and a MRI dataset (`ds003483`). These datasets are only meant to learn how to use ancpBIDS, and are not expected to be used in any kind of research.

```

With the path to your dataset and `BIDSLayout()` you can create the [in-memory graph]((../extra/inmemory)) of it, from where you can easily retrieve information. 

```bash
from ancpbids.pybids_compat import BIDSLayout
layout = BIDSLayout(dataset_path)
```

The output (`layout` object) contains both the loaded `dataset` and the `schema`.

    
### Alternative way to load a dataset

Alternatively, you may also use the function `load_dataset()` along with path to your dataset to load the dataset. 

````{tab-set}
```{tab-item} Simple loading

    from ancpbids import load_dataset
    dataset = load_dataset(dataset_path)
    #print(dataset)
    #{'name': 'ds003483'}

```

```{tab-item} Loading with DatasetOptions

    from ancpbids import load_dataset, DatasetOptions
    dataset = load_dataset(dataset_path, DatasetOptions(ignore=False, infer_artifact_datatype=True))
    #print(dataset)
    #{'name': 'ds003483'}

```
````



## Next section
Now that your dataset is loaded and mapped into a layout, you're ready to start extracting useful information from it. In the next sections, we'll show how to:
* [Basic Queries](https://ancplaboldenburg.github.io/ancpbids_documentation/query/basic.html) using common entities, like subjects and tasks.
* [Adanced Queries](https://ancplaboldenburg.github.io/ancpbids_documentation/query/advanced.html) using specific files using parameters.
* [Retrieve metadata](https://ancplaboldenburg.github.io/ancpbids_documentation/query/metadata.html) from sidecars and filenames.

