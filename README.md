![Data package](header.png)

This repo contains the results data for Round 2 of [EGFR Protein Design Competition](https://foundry.adaptyvbio.com/competition), hosted by Adaptyv Bio in partnership with Polaris and Dimension.

----

📊 Processed binding affinity characterization data and sequence similarity metrics:


> [Results folder](results) 


🔬 Kinetic curves and raw data:


> [https://api.adaptyvbio.com/storage/v1/object/public/egfr_design_competition_2/package.zip](https://api.adaptyvbio.com/storage/v1/object/public/egfr_design_competition_2/package.zip)


🧱 AlphaFold2 Structure predictions (.pdb) for all 400 selected designs:

> [https://api.adaptyvbio.com/storage/v1/object/public/egfr_design_competition_2/structure_predictions.zip](https://api.adaptyvbio.com/storage/v1/object/public/egfr_design_competition_2/structure_predictions.zip)

🌐 Sequence embeddings from a variety of models (ESM C, ESM2, Saprot, Protek) for all submissions:

> [https://api.adaptyvbio.com/storage/v1/object/public/egfr_design_competition_2/embeddings.zip](https://api.adaptyvbio.com/storage/v1/object/public/egfr_design_competition_2/embeddings.zip)

# Methods

## Metrics

More details on the metrics that were used for ranking the sequences can be found in the [metrics repo](https://github.com/adaptyvbio/competition_metrics).

### AlphaFold2 Scores

Two of the metrics that we used for scoring the designs computationally are derived from AlphaFold2 predictions. To calculate them, we began by generating a structure prediction using ColabFold (with 5 models, 3 recycles, 3 seeds, with templates and without initial guess). The top-ranked model was selected for each design.

To get **PAE interaction**, The Predicted Aligned Error (PAE) of the top-ranked prediction was then averaged across residue pairs, where one residue belongs to the target and the other to the binder, as done [here](https://github.com/nrbennet/dl_binder_design/blob/cafa3853ac94dceb1b908c8d9e6954d71749871a/af2_initial_guess/predict.py#L197).

The second metric, **ipTM**, is predicted by the model directly.

Here we also show the predicted Local Distance Difference Test (**pLDDT**) scores, averaged over residues of the binder chain.

### ESM PLL

The third metric used in the ranking is **ESM2 PLL** (Pseudo Log Likelihood). We use the `esm2_t33_650M_UR50D` model for the calculation and we do not normalize by the length of the sequence.

### Sequence Similarity Check

We checked each sequence against several sequence databases. As part of the initial competition rules, only proteins that were at least 10 amino acids (AA) away from a published sequence were considered valid and counted in the final leaderboard. The results of that similarity search are stored in the results folder. The **similarity check** metric is calculated as `identity * coverage`, where:

• **Identity** is the highest percentage of matching amino acids between a subsequence of the query and a subsequence of the database entry.

• **Coverage** is the fraction of the query sequence that aligns with the database entry.

Proteins with less than 10 amino acid distance to a database entry were excluded from the competition. A `similarity_check` value of “null” indicates that no matches were found in any of the the databases.

The databases that we checked are SwisssProt, THPdb, USPTO, sequences from the first round of the competition and binders designed by [Cao et al. (2022)](https://www.nature.com/articles/s41586-022-04654-9). The scripts can be found in the [first round data package repo](https://github.com/adaptyvbio/egfr_competition_1/tree/main).

## Experimental Workflow

### DNA Design

The submitted protein sequences were reverse-translated, and the corresponding DNA sequences were optimized using Adaptyv's internal pipeline. This process considered several parameters, including optimal codon usage for cell-free systems, mRNA secondary structure stability, and synthesizability factors. Additionally, non-coding regions at the 5' and 3' ends, optimized for cell-free expression, were incorporated into the coding sequences. Suitable gene constructs were successfully generated for all submitted protein sequences.

### Protein Synthesis

Protein synthesis was carried out using an optimized cell-free expression system, suitable for a wide range of proteins. The template DNA was added, and protein expression was conducted over a defined period. During the competition, at least two expression batches were performed for each sequence entry, with some sequences tested up to four times under varying conditions. Protein synthesis success was assessed via a label-free quantification assay. Sequences that yielded less than 0.02 µg/mL of protein were excluded from further experimental characterization.

### Binding Assay

The binding assay was conducted using Bio-Layer Interferometry (BLI), a label-free technology for biomolecular interaction measurement. A multi-cycle kinetic assay was performed against the target antigen. Expressed ligands were immobilized on the probe surface using tag-specific chemistry, and several concentrations of the antigen (ranging from 1000 nM to 10 nM) were flowed over the probe. The experiments were performed in duplicate using a HBS-T buffer with 0.5% BSA at 25°C.

### Data analysis

The binding signals were baseline-corrected and fitted using a 1:1 binding model across all tested concentrations for each replicate. This approach allowed us to extract the kinetic rates (association and dissociation) and calculate the affinity constants (KD) for each ligand. The predicted binding curves were generated based on the fit parameters, ensuring an accurate representation of the interaction dynamics. In cases where the maximum signal fell below the quantifiable threshold, or when the interaction kinetics were too fast relative to the device's temporal resolution, we employed equilibrium analysis to estimate the dissociation constant (KD). Each experimental replicate was analyzed independently.

# License

All code is licensed under Apache 2, all data is licensed under the [ODbL](https://opendatacommons.org/licenses/odbl/). Contact igor@adaptyvbio.com or any other responsible at Adaptyv if you require a different license.
