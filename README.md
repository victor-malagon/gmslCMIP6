# gmslCMIP6
This repository contains several scripts that were used for the submitted manuscript **Projecting Global Mean Sea-Level Change Using CMIP6 Models** by Hermans et al. (2020), which is premised on global thermal expansion (GTE) and global surface air temperature (GSAT) data from CMIP6 models. The repository can be used for pulling CMIP6 data (in this case, global mean thermosteric sea-level change 'zostoga', gridded surface air temperature 'tas' and atmosphere cell area 'areacella') from ESGF using WGET, preprocessing raw (monthly mean intended, but possibly also other frequencies) 'zostoga' and 'tas' data, including time-merging, dedrifting and calculating area-weighted means, and analyzing the preprocessed data. The underlying data of the manuscript will be made available at: http://doi.org/10.4121/12958079.

# Downloading with WGET
Instructions for running the WGET scripts can be found here: https://esgf.github.io/esgf-user-support/user_guide.html#download-data-from-esgf-using-wget. For example, using

```
bash wget-20200715133532_zostoga.sh
```

will pull all 'zostoga' variant files with realization=1 from ESGF and store them per variable per model, as indicated by the download structure variable in the Search URL. Note that a 5 second delay can be built in with 'sleep 5' between each download, to avoid overasking certain ESGF nodes. The script may require a few iterations to completion, depending on server response and hiccups for particular models. Similar scripts may be generated by issueing the RestFull API URL ('Search URL' in the WGET scripts) in your browser. 

# Converting 'tas' into GSAT
is done using *cmip6_get_areawmean.py*, which requires gridded data as well as area cell weights (CMIP6 'areacella' (atmosphere), 'areacello' (ocean)), which can be downloaded using WGET as well.

*to-do: develop script that iteratively does this for each model*

# Dedrifting 'zostoga'
The dedrifting scripts loop over each model and experiment. The input is expected to be time-merged (e.g., using *cmip6_merge_raw_timechunks.py*), and organized per model directory. Two flavours of dedrifting using piControl experiments are available: 1) linear dedrifting, which fits a linear polynomial to the full piControl time series, and subtracts that from the historical and SSP experiments, 2) quadratic dedrifting, which fits a quadratic polynomial to the full piControl time series, and subtracts the **overlapping** part of the piControl run from historical and SSP experiments. 2) requires additional information on the branch times of historical in piControl, and ssps in historical. This information is available from metadata, but is for some models inconsistent, and so less generic than 1). 1) yields small differences to 2) compared to any forced signal over the 21st century (Hermans et al., 2020, submitted to GRL).  

*to-do: upload scripts to store data in NetCDFs, this also includes routines to reference against baseyears, logic to pick from variants, etc.*

# Analysis example
The GTE and GSAT data (available at http://doi.org/10.4121/12958079, or collected through the above downloading and preprocessing procedure), can conveniently be plotted using xarray. *plot_GTE_GSAT_CMIP5_CMIP6.py* demonstrates this for CMIP5 and CMIP6 data, and produces Figure 1 of the manuscript. 

---
Required Python packages: xarray, numpy, fnmatch, cdo
