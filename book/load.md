# Loading a BIDS Dataset
Before you can query or modify your data, you need to make your dataset available to ancpBIDS. This involves two steps: fetching the dataset and creating a layout to represent its structure.


## Fetch an existing BIDS dataset

ancpBIDS is able to find and load specific files from your PC. With ´fetch_dataset()´,ancpBIDS will be able to fetch it even if you haven't unzip your dataset. The output, ´dataset_path´, will contain the local path to it and within your PC folder _'~/.ancp-bids/datasets'_ you will find the zip file (f.e. ds005-testdata.zip) and the content extracted. If you run the code again, it won't create unnecessary copies.


````{tab-set}
```{tab-item} MEG

    from ancpbids import utils
    dataset_path = utils.fetch_dataset('ds005')

```

```{tab-item} MRI

    from ancpbids import utils
    dataset_path = utils.fetch_dataset('ds003483')

```
````


## Creating a Layout
Once you fetch the path to our dataset folder, we can use ´BIDSLayout()´ to create the in-memory graph of your dataset, from where you can easily retrieve information. Both the dataset and the schema are accessible through the layout object.


    from ancpbids.pybids_compat import BIDSLayout
    layout = BIDSLayout(dataset_path)


  
```{admonition} In-memory graph?
:class: tip
If you want to learn more how ancpBIDS uses the BIDS specification to build the in-memory graph representation (and what exactly is a in-memory graph representation), [follow this link](https://alexisbaxman.github.io/ancpbids_documentation/extra/inmemory.html).

```
    
### Load a dataset
Thanks to BIDSLayout your dataset it's already loaded within the layout variable. But you may also use the function ´load_dataset()´ along with the dataset_path you queried before to load the dataset. 


```{warning}

If you load your dataset with load_dataset(), you won't have access to the BIDS Schema. Therefore, we suggest you to use BIDSLayout instead.

```

````{tab-set}
```{tab-item} Simple loading

  from ancpbids import load_dataset
  dataset = load_dataset(dataset_path)
  #print(dataset)
  #{'name': 'ds003483'}

```

```{tab-item} Loading with DatasetOptions

  from ancpbids import load_dataset, DatasetOptins
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

