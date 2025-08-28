# Installation Guide

Let's get started with ancpBIDS.
To install ancpBIDS, run the following command in your terminal:

```bash
pip install ancpbids
```

It can also be upgraded:

```bash
pip install --upgrade ancpbids
```

## Virtual environments
Before starting with the tutorial, we want to briefly explain you about virtual environments and containerization.

Virtual environments create isolated and self-contained workspaces, allowing us to manage project-specific dependencies separated from system-wide installation. This isolation has several benefits:
- **Avoid dependency conflicts:** prevents interferences between project-specific and system-wide dependencies, such as common errors related to version mismatches.
- **Transparency and Open Science:** Ensures that others can replicate your results and reproduce your analysis reliably.

<img src="../static/environment.jpg" alt="bids-schema" width="600px" align="center">

### Create and activate your virtual environment
The following steps should be run from a **Bash terminal** (e.g.,  your Linux shell, WSL, macOS Terminal, or Git Bash). These are not Python commands.

1. Navigate to the directory where you want to create the environment using the `cd` command in the terminal.

```bash
cd /path/to/your/project
```

3. Create the virtual environment:

```bash
python3 -m venv <your_environment_name>
```

4. Activate the virtual environment:

```bash
source /path/to/environment/bin/activate
```

# Next section
In the next section we will show how to use ancpBIDS to fetch datasets and perform basic queries.




