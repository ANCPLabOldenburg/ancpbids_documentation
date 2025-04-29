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
Once you fetch the path to our dataset folder, we can use ´BIDSLayout()´ to create the _[in-memory graph](https://alexisbaxman.github.io/ancpbids_documentation/extra/inmemory.html)_ of your dataset, from where you can easily retrieve information. 

    from ancpbids.pybids_compat import BIDSLayout
    layout = BIDSLayout(dataset_path)

Both the dataset and the schema are accessible through the layout object.
    
:::{note} 

You may also use the function ´load_dataset()´ along with the dataset_path you queried before to load the dataset. This one won't include the _schema_.
      from ancpbids import load_dataset
      dataset = load_dataset(dataset_path)
      # print(dataset)
      # {'name': 'ds003483'}
      
:::

## Next section
Now that your dataset is loaded and mapped into a layout, you're ready to start extracting useful information from it. In the next sections, we'll show how to:
* Query common entities, like subjects and tasks. [Follow this link](https://ancplaboldenburg.github.io/ancpbids_documentation/query/basic.html)
* Query specific files using parameters. [Follow this link](https://ancplaboldenburg.github.io/ancpbids_documentation/query/advanced.html)
* Retrieve metadata from sidecars and filenames. [Follow this link](https://ancplaboldenburg.github.io/ancpbids_documentation/query/metadata.html)

