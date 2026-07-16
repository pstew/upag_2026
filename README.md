## UPAG Proteomics Data Analysis Workshop July 2026

### Slido link:

https://app.sli.do/event/a8YC6SE7EhBwaoa1NQzWJb

### Link to this Repository:

https://github.com/pstew/upag_2026

### R/RStudio download

https://posit.co/download/rstudio-desktop/

### Materials

- Intro
- GitHub
  - What it is
  - Getting files for today
- Getting started with R: https://monashbioinformaticsplatform.github.io/r-intro/start.html
- Set up project in a folder + new RStudio project
	- Importance of staying organized
	- Project folder structure
		- source files
		- data
		- scripts
		- work (or results)
	- More reading: https://pmc.ncbi.nlm.nih.gov/articles/PMC2709440/
- Let's look at DIA_LFQ_ProteinAbundanceForRscriptAnalysis.xlsx
	- For 100% reproducibility, we want to minimize data handling in Excel because Excel is prone to errors.
	- File names are important (and also contain important data)
		- Where it came from/what it is/what it is for
	- Questions to ask: what am I looking at? Are there the right number of samples? How many features (e.g. proteins)? Has the data been manipulated in any way? What annotations are there? Experimental groups? Column names need a mapping table (e.g.  sample_01, sample_02, sample_03) and do not tell us what group they belong to?
		- Has both column annotation and row annotation. This is important "meta data".
		- There are twenty samples across four conditions: KO_TP, KO_baseline, WT_TP, WT_baseline.
	- "Group Labels" are protein annotations. These look like Uniprot annotations (https://www.uniprot.org), which is pretty standard protein database for proteomics experiments. 
		- When in doubt about annotations, just check.
			- https://www.uniprot.org and https://www.uniprot.org/id-mapping
			- https://www.genecards.org
		- Cannot tell what the identity is just by looking at them. Eventually need to be mapped to gene symbols.
		- Be careful with Uniprot "Entry Name", which look like gene symbols but are not. Will cause issues when doing downstream analyses and looking them up in the literature.
		- Have ...-1, ...-2, which indicates isoforms.
			- Isoforms not always included in reference library
	- 7409 features (e.g. proteins).
		- A good number for shotgun proteomics.
		- Fewer features than expected can indicate sample or sample prep issues.
		- Very few features means it is likely a targeted proteomics experiment (and I might not be on the same page as to what kind of experiment was performed).
	- Data is not raw intensities because there are decimals. This tells me the data has likely been normalized or transformed.
		- File name confirms it: "LFQ"
			- LFQ is label free quantification, which normalizes intensity values across runs to reduce technical variation. It uses pairwise peptide ratio comparisons to estimate protein-level abundance.
			- Not standarized! LFQ is a general concept, not a single method. MaxQuant LFQ (MaxLFQ) is different than MSFragger (via IonQuant).
		- Missing values are blank and have not been imputed (missing values replaced with some value or different values).
			- Good for analyses in R. R will treat blanks as NA, which is a special value representing missing (or undefined) data. 
			- We expect some missingness in mass spec data. 
				- Mass spec data that does not have missingness indicates it was imputed or possibly run with TMT (which tends to have very little missingness).
  		- Fix typo in H3.
	- Let's separate our column annotations to make separate meta data. 
		- Do not need to do it now, but we could also do this for row annotations since we generally have a lot of information associated with each protein/peptide in proteomics experiments.
		- A text editor like https://www.sublimetext.com can really help speed up basic text manipulation. This could mostly be done in RStudio as well.
		- Critically important for understanding what data we have and staying organized in our analysis.
			- Avoids "hard coding" of samples.
			- Easy to add additional data later as it becomes available.
		- We need unique column names. R will see duplicated column names and handle them implicitly, but this is can be prone to errors and lead to confusion for beginners. 
			- Better to define them explicitly.
		- Save with descriptive-name_date in data folder as a tab delimited text file. 
			- Why tab delimited files are generally a good idea.
			- Use yyyy-mm-dd date format for sorting. 
	- Delete the extra column headers in the experimental data since this information is saved in the meta data file. 
		- In R we will define the first row as the column header so we can refer to specific samples/conditions.
		- Save with descriptive-name_date in data folder as a tab delimited text file. 
	- Switch back to RStudio (see R markdown file).
