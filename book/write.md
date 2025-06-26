## Write Derivatives
´write_derivative()´ Writes the provided derivative folder to the dataset. Note that a ‘derivatives’ folder will be created if not present. Optionally, you may also use the ´DatasetOptions´ class to set your preference in the handling of writing a derivative to your file system.

    from ancpbids import write_derivative
    
    target_dir = '/path/to/your/traget/directory'
    write_derivative(dataset, target_dir)

    # TODO: provide a more elaborative example using write_derivative() (deprecated) and artifact.write() (preferred)
