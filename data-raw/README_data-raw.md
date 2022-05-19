This folder contains raw data and the scripts to download and pre-process the data.

## extract_GTI.R
Script to extract global topographic index (GTI) values at fluxnet locations. 

## extract_gldas.R
Script to extract GLDAS variables and put them in the right format. Designed to be run site-by-site on a cluster.

### Instructions to download GLDAS data on ETHZ's HPC cluster (Euler):

wget --load-cookies ~/.urs_cookies --save-cookies ~/.urs_cookies --auth-no-challenge=on --keep-session-cookies --content-disposition -i ~/data/LDAS/subset4.txt

Where 'subset4.txt' is a list of download links of the files as produced on the LDAS website. 


1. Load modules:

env2lmod
module load gcc/6.3.0 r/4.0.2 gdal/3.1.2 udunits2/2.2.24 proj/4.9.2 geos/3.6.2 python_gpu/3.8.5 netcdf/4.4.1.1
module load eth_proxy


2. Define vector of fluxnet sites where SM is consistent (see Methods):

#!/bin/bash
declare -a vec_site=("AR-SLu" "AR-Vir" "AT-Neu" "AU-Ade" "AU-ASM" "AU-Cpr" "AU-Cum" "AU-DaP" "AU-DaS" "AU-Dry" "AU-Emr" "AU-Fog" "AU-Gin" "AU-GWW" "AU-How" "AU-RDF" "AU-Rob" "AU-Stp" "AU-Tum" "AU-Wac" "AU-Whr" "AU-Wom" "AU-Ync" "BE-Bra" "BE-Lon" "BE-Vie" "BR-Sa3" "CH-Cha" "CH-Dav" "CH-Fru" "CH-Lae" "CH-Oe1" "CH-Oe2" "CN-Cng" "CN-Dan" "CN-Din" "CN-Du2" "CN-Qia" "CZ-BK1" "CZ-BK2" "CZ-wet" "DE-Akm" "DE-Geb" "DE-Gri" "DE-Hai" "DE-Kli" "DE-Lkb" "DE-Obe" "DE-RuR" "DE-RuS" "DE-Seh" "DE-SfN" "DE-Spw" "DE-Tha" "DK-Fou" "DK-NuF" "DK-Sor" "ES-LgS" "ES-Ln2" "FI-Hyy" "FI-Jok" "FI-Sod" "FR-Fon" "FR-Gri" "FR-LBr" "FR-Pue" "GF-Guy" "IT-BCi" "IT-CA1" "IT-CA2" "IT-CA3" "IT-Col" "IT-Cp2" "IT-Cpz" "IT-Isp" "IT-La2" "IT-Lav" "IT-MBo" "IT-Noe" "IT-PT1" "IT-Ren" "IT-Ro1" "IT-Ro2" "IT-SR2" "IT-SRo" "IT-Tor" "JP-MBF" "JP-SMF" "NL-Hor" "NL-Loo" "NO-Adv" "RU-Fyo" "SD-Dem" "SN-Dhr" "US-AR1" "US-AR2" "US-ARb" "US-ARc" "US-ARM" "US-Blo" "US-Cop" "US-GLE" "US-Ha1" "US-Los" "US-Me2" "US-Me6" "US-MMS" "US-Myb" "US-Ne1" "US-Ne2" "US-Ne3" "US-ORv" "US-PFa" "US-SRG" "US-SRM" "US-Syv" "US-Ton" "US-Tw1" "US-Tw2" "US-Tw3" "US-Tw4" "US-Twt" "US-UMB" "US-UMd" "US-Var" "US-WCr" "US-Whs" "US-Wi0")


3. We can then run the algorithm:

for val in "${vec_site[@]}"; do
  echo $val
  bsub -W 1:00 -u fgiardina -J "job_name $val" -R "rusage[mem=2000]" "Rscript --vanilla extract_gldas.R $val"
done