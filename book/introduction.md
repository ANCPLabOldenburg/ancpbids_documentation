# Introduction to ancpBIDS

ancpBIDS was developed by **Erdal Karaca** as part of his Master Thesis (2022) within the **Applied Neurocognitive Psychology (ANCP) Lab**. In agreement with some members of the **PyBIDS** community, ancpBIDS offers similar I/O (Input/Output) handling, focusing on remaining lightweight and ensuring backwards compatibility with it. 

* **Significant performance optimization.** thanks to its **in-memory graph representation**, ancpBIDS efficiently manages the loading process of datasets across multiple layers. A benchmark comparing dataset loading and querying performance showed that ancpBIDS performs efficiently across various dataset sizes.

<img src="../static/benchmark.PNG" alt="bids-benchmark" width="400px">



* **Maintainable and clean implementation.** A modular structure helps to control the complexity and minimizes transitive dependencies, making ancpBIDS lightweight and attractive for third-party integration. This design also encourages community contributions and future extensibility.

* **Support for multiple BIDS schema versions.** ancpBIDS dynamically adapts to different BIDS versions based on the dataset’s version declaration.

## What is BIDS? <img src="../static/bids.jpg" alt="bids-logo" width="200px">

*"Neuroimaging experiments result in complex data that can be arranged in many different ways, and for a long time, there was no consensus on how to organize and share data obtained in neuroimaging experiments. **Brain Imaging Data Structure (BIDS)**, describes a simple and easy to adopt way of organizing neuroimaging and behavioral data"* (Gorgolewski et al., 2016; Niso et al., 2018). 

<img src="../static/bids-order.jpg" alt="bids-order" width="600px" align="center">



The **[BIDS Specification](https://bids-specification.readthedocs.io/en/stable/)** defines the rules for data organizing and naming conventions. It is continuously updated thanks to community efforts. To ensure that the Specifications are implemented consistently, BIDS provides **[BIDS Schema](https://bids-specification.readthedocs.io/en/stable/appendices/schema.html)**, a machine readable representation written in YAML format. The BIDS Schema describes objects (definition of BIDS concepts), rules (rules of file path names and contents) and meta (context to which rules can be applied).

<img src="../static/bids-schema.png" alt="bids-schema" width="200px">



You can find more information the BIDS Specification on their [official BIDS webpage](https://bids.neuroimaging.io/).


```{admonition} See also

If you want to learn more how ancpBIDS uses the BIDS specification to build the in-memory graph representation, [follow this link](guide/inmemory.md).

```

## Next section
In the next section, we'll walk through the installation and basics for the tutorial.


