# FACT-Fully-Automatic-Codification-Tool

FACT is a Python tool for automatic semantic coding of Property Listing Task (PLT) data. It reads an input file containing participant-generated properties for multiple concepts, embeds those properties using a multilingual transformer, groups them using a community-detection approach, evaluates coverage for each concept, and selects the best clustering solution based on a target coverage value.


## Input File Structure

The input file must be a CSV containing one row per property produced by one participant for one concept. The file must include exactly the following columns:

- concept: the concept label.

- participant: an identifier for the participant (numeric or string).

- property: the raw text of the property.

- there are three Input Files that allow following the results presented in the paper, one for each CPN: CPN27_raw.csv, CSBL_raw.csv, and Sit_Conc_raw.csv (see paper for further details).


##  Running FACT

Steps to use the tool:

1.  Open the FACT notebook (FACT_2.5.ipynb for data in Spanish; FACT_2.5_e.ipynb for data in English) in Jupyter or Google Colab. The difference in language is basically due the light cleaning block (e.g., different stop-words).

2. Place the input file in an accessible path. For example: 
   
   ```
   /content/CPN27_raw.csv
   ```

3. At the top of the notebook, update the following variables:
    ```
    CSV_PATH = "<path to your input file>"

    OUTPUT_PREFIX = "<prefix for all output files>"
    ```
    Example:
    ```
    CSV_PATH = "/content/CPN27_raw.csv"

    OUTPUT_PREFIX = "/content/CPN27_FACT"
    ```
4. Run the entire FACT code cell. This executes the complete pipeline:

   - Installs required libraries

   - Reads and cleans the input file.

   - Produces text-normalized versions of properties.

   - Embeds each property using a transformer model.

   - Builds a similarity graph using k-nearest neighbors.

   - Runs a grid search over clustering parameters.

   - Computes coverage metrics for each concept.

   - Selects the best parameter combination.

   - Saves all output files.

## Output Files


FACT produces the following files, all sharing the chosen OUTPUT_PREFIX:

1. `<prefix>_codes.csv`

    A full table including:

    - concept

    - participant

    - property

    - property_norm

    - code

    This file contains one row per raw property with the assigned code.

2. `<prefix>_metrics_per_concept.csv`

    Contains per-concept statistics:

    - concept

    - U: number of raw properties

    - T: number of participants

    - Q1: codes mentioned once

    - Q2: codes mentioned twice

    - coverage: estimated coverage

    - n_codes: number of unique codes for the concept

3. `<prefix>_code_sizes.csv`

    Contains, for each concept–code pair:

    - concept

    - code

    - n_properties

4. `<prefix>_summary.json`

    A summary of:

    - best parameter settings (k, sim_min, resolution)

    - best loss value

    - average coverage

    - total number of codes

    - total number of rows

5.  `<prefix>_time.json`
    
    A summary of the time on the:

    - Standardization process
    
    - Embedding of the properties
  
    - Hyperparameter search
    
    - Total time of the algorithm


6. `<prefix>_overall_cluster_size_hist.png`

    A histogram showing global cluster size distribution    

7. `<prefix>_per_concept_top_codes.pdf`

    A multi-page PDF with bar charts of the largest codes for each concept (singletons are not shown here)

##  Notes

- FACT automatically searches for the parameter set that produces coverage values closest to the target.

- The tool assumes that the input file follows the exact structure described above.

- All outputs are saved automatically once the run is completed.

