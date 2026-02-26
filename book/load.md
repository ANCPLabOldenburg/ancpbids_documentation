# Load a BIDS Dataset

With ancpBIDS you don't really load your dataset, but to make it available to ancpBIDS to create an [in-memory graph](../extra/inmemory). There are several ways to load a BIDS dataset just using a `dataset path`.

## Basic Loading

```bash
from ancpbids.pybids_compat import BIDSLayout
layout = BIDSLayout(dataset_path)
```

The output (`layout` object) contains both the loaded `dataset` and the `schema`.



```{admonition} Don't have a Dataset?
:class: dropdown

In case you don't have a BIDS compliant dataset, you can download a test dataset from our [github](https://github.com/ANCPLabOldenburg/ancp-bids-dataset) using `fetch_dataset()`. 

    from ancpbids import utils
    dataset_path = utils.fetch_dataset('ds005')

The output variable, `dataset_path`, will contain the local path to your dataset.

We offer an MEG dataset (`ds005`) and a MRI dataset (`ds003483`). These datasets are only meant to learn how to use ancpBIDS, and are not expected to be used in any kind of research.

```

## Advance Loading

You can also use `load_dataset` to load your dataset, it requires you to read the schema separatedly but you can combine it with `DatasetOptions` for more advance options.


````{tab-set}
```{tab-item} Simple loading

    from ancpbids import load_dataset
    dataset = load_dataset(dataset_path)
    schema = dataset.get_schema()
    print(dataset)

    #{'name': 'ds003483'}

```

```{tab-item} Infer artifact data type

* `lazy_loading` [default: True]: If True, file contents will be loaded only when accessed, not during data initialization. This can significantly improve memory usage and loading performance for large datasets. If False, `load_dataset` will immediately load all metadata file contents into memory during dataset intialization
* `inter_artifact_datatype` [default: False]: if True, it will determine the datatype of an Artifact, for example reading the directory path (`/func/`). It may have a negative performance. 
* `ignore` [default: False]: if True, if there's a .bidsignore file is available, all matching files and folders won't be added to the in-memory graph.


```bash
    from ancpbids import load_dataset, DatasetOptions
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

