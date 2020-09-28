# gmslCMIP6
This repository contains several scripts that were used for the submitted manuscript **Projecting Global Mean Sea-Level Change Using CMIP6 Models** by Hermans et al. (2020). The repository can be used for pulling CMIP6 data (for example, gridded surface air temperature 'tas' and global mean thermosteric sea-level change 'zostoga') from ESGF using WGET, preprocessing raw (monthly mean, but probably also other frequencies) 'zostoga' and 'tas' data, including time-merging, dedrifting and calculating area-weighted means, and analyzing the preprocessed data. The underlying data will also be made available at: http://doi.org/10.4121/12958079.

# Downloading
Instructions for running the WGET scripts can be found here: https://esgf.github.io/esgf-user-support/user_guide.html#download-data-from-esgf-using-wget. For example, using

```
bash wget-20200715133532_zostoga.sh
```

will pull zostoga files from ESGF and store them per variable per model. Note that a 5 second delay has been built in between each download, to avoid overasking ESGF nodes. The script may require a few iterations before the download is complete, depending on server response. Similar scripts may be generated by issueing the RestFull API URL ('Search URL') in your browser. 

*to-do: upload scripts for downloading 'tas'*

# Converting 'tas' into GSAT
is done using *cmip6_get_areawmean.py*, which requires gridded data as well as area cell weights (CMIP6 'areacella' (atmosphere), 'areacello' (ocean)), which can be downloaded similarly to zostoga using WGET.

# Dedrifting 'zostoga'
Expects input to be time-merged (e.g., using *cmip6_merge_raw_timechunks.py*), and organized per model directory. Two flavours of dedrifting using piControl experiments are available: 1) linear dedrifting, which fits a linear polynomial to the full piControl time series, and subtracts that from the historical and SSP experiments, 2) quadratic dedrifting, which fits a quadratic polynomial to the full piControl time series, and subtracts the **overlapping** part of the piControl run from historical and SSP experiments. 2) requires additional information on the branch times of historical in piControl, and ssps in historical. This information is available from metadata, but is for some models inconsistent, and so less generic than 1). 1) yields small differences to 2) compared to any forced signal over the 21st century (Hermans et al., 2020, submitted to GRL).  


Required Python packages: xarray, numpy, fnmatch, cdo
