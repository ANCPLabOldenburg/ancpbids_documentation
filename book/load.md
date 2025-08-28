# Load a BIDS Dataset

Before you can query or modify your data, you need to make your dataset available to ancpBIDS.

```{admonition} Don't have a Dataset?
:class: dropdown

In case you don't have a BIDS compliant dataset, you can download a test dataset from our [github](https://github.com/ANCPLabOldenburg/ancp-bids-dataset) with `fetch_dataset()`. The output variable, `dataset_path`, will contain the local path to your dataset.

````{tab-set}
```{tab-item} MEG
    from ancpbids import utils
    dataset_path = utils.fetch_dataset('ds005')
```{tab-item} MRI
    from ancpbids import utils
    dataset_path = utils.fetch_dataset('ds003483')
````

You can find the downloaded content in (this may be different depending on your operating system):

```bash
home/user/.ancp-bids/datasets
```

These datasets are only meant to learn how to use ancpBIDS, and are not expected to be used in any kind of research. 
 

```

Once you have your dataset available locally on your PC, we can use ´BIDSLayout()´ to create the in-memory graph of it, from where you can easily retrieve information. Both the dataset and the schema are accessible through the layout object.

```bash
from ancpbids.pybids_compat import BIDSLayout
layout = BIDSLayout(dataset_path)
```
  
```{admonition} In-memory graph?
:class: tip

If you want to learn more how ancpBIDS uses the BIDS specification to build the in-memory graph representation (and what exactly is a in-memory graph representation), [follow this link](../extra/inmemory).

```
    
### Load a dataset
**Thanks to BIDSLayout your dataset it's already loaded within the layout variable.**

Alternatively, you may also use the function ´load_dataset()´ along with the dataset_path you fetched before to load the dataset. 


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

