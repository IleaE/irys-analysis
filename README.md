# irys-duf1220

irys-duf1220 is a set of tools for utilizing data produced by the Irys system to analyze structural variation in DUF1220 containing genes

# Getting Started
You will want to run 2 types of analysis.  

You will want to analyze the results of **molecule to reference** alignments, with multimatch on.  

You will also want to analyze the **molecule to contig - contig to reference** alignments, in order to recover information for certain genes (e.g. NBPF19) for which very little information is recovered with the molecule to reference approach.  

# Analyze the data
All of the necessary code has been packaged into one script, _irys-analyzer.sh_ with an accompanying _config.sh_ script to set various parameters and file paths.  The script will automatically analyze both HLS region and CON1 region variation. Depending on the **alignment_type** you set in the _config.sh_ script, the _irys-analyzer.sh_ script will run the appropriate analysis. 

1) Ensure parameters in the config.sh file are set as desired
- alignment type (one of: MolRef, MolContig, or ContigRef)
- sample directory
- desired name of results folder
- file name setup
- number of samples
- conf_spread
  - the **conf_spread** parameter defines the minimum difference between the maximum confidence score and other confidence scores for the molecule for the alignment to be retained
- link_dist
  - the **link_dist** parameter controls the size of the "bin" in the peak-caller.sh script 
- min_mols_in_cluster
  - the **min_mols_in_cluster** parameter defines how many molecules you want in each cluster in order to call the cluster as an SV 

2) Run the script:
```
bash irys-analyzer.sh
```
**Notes:**
The script automatically generate two sets of results, one in which molecules with a nick between the aligned con2 and con3 nicks are filtered out (the "adj-mols-only" folder within the main results folder), and a set of results in which that filter is not applied (the main results folder)  

File structure:
- For MolRef data, the script is set up to operate on a single folder that contains all of the files (xmap, r.cmap, and q.cmap) for every sample



# Possibly Deprecated below this header 
### Analyze the molecule to contig - contig to reference data
There are two steps, the first is to analyze the contig to reference data.  The second is to analyze the molecule to contig data.  The results of both of these steps are files that are equivalent to the "con2-con3-distance" files produced by the "standard" analysis.  You run all the same downstream steps that you do with the regular analysis. 

This code runs both the contig to reference (scripts/run-contig-to-ref.sh) and mols to contig (scripts/run-mols-to-contigs.sh) scripts.  
- First, modify the script to point to the correct data folder, specify the results folder and results file names
```scripts/run-contig-to-ref-mols-to-contigs.sh```



##### File name formatting  
If the file names use underscore separators, convert them to dashes with _renamefiles.sh_.  First move the script into the folder with the files that need the name change,then cd into that directory and run as shown below.  Then move the script back to the scripts directory. Note: if you later intend to open the xmap with the changed file name, you'll need to revert the file names of the r.cmap and q.cmap files to match their original formatting - if you do not, then IrysView will throw an error.
  
 ```
 bash renamefiles.sh
 ```

