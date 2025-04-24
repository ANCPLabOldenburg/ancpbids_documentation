# Writting a New Dataset

Once you've queried and worked with your BIDS dataset, you may want to save processed data or generate derivative datasets. Unlike PyBIDS, ´ancpBIDS´ supports **writing new datasets directly from its [in-memory graph](https://ancplaboldenburg.github.io/ancpbids_documentation/extra/inmemory.html).**
When calling `ancpbids.save_dataset()`, the in-memory graph materializes as a new folder on disk. This is done "schema-aware": following the syntax and the semantic BIDS specification, such as naming Artifacts with correct key-value pairs. The new folder will contain the [dataset_description.json](https://alexisbaxman.github.io/ancpbids_documentation/extra/inmemory.html#the-model-of-a-bids-dataset), with the field `BIDSVersion` derived directly from the schema.

![dataset_description_json](../static/dataset_description_json.png)

```{note}
For each subject, a separate folder is created, and **Artifacts are named automatically:** the key-value information (such as *'sub-09'*) is inferred from the folder structure. This writting functionally allows to export fully valid BIDS derivatives dynamically and automatic within pipelines. 

```




