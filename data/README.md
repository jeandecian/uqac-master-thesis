# Research Datasets

This directory contains all the datasets used for the Master's Thesis. It is strictly organized into two subdirectories to maintain data integrity and reproducibility.

## Structure

* `/raw/`: Contains the original data as received for analysis. This is the initial, immutable dataset committed by the collaborators. While this data may reflect initial formatting applied during the collection stage, it serves as the definitive, primary source for the thesis. These files are read-only; modification is strictly prohibited.
* `/processed/`: Contains intermediate files derived from the raw data. This includes steps to prepare the datasets for analysis, such as cleaning, aggregation, normalization, and feature extraction. These datasets are directly consumed by the scripts in the `/code/` directory.
