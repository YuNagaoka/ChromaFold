## Hi-C normalization 

This page is for preparing [HiC-DC+](https://github.com/mervesa/HiCDCPlus) normalized Hi-C libraries for training. 

### **Step 1**. Generate HiC-DC+ normalized z-score files

- **Input:** HiC-DC+ requires assembly and resolution specific feature files for regression. Ready-to-use genomic features have been uploaded to [feature files](https://drive.google.com/drive/folders/1084P15MIrYeS13_ynpx2fbu11HTDszOt?usp=sharing). Please download them for normalization. 
- **Software and tools:** please download [Straw](https://github.com/aidenlab/straw) for data extraction from `.hic` format. A ready-to-use `straw.cpp` can also be downloaded from [google drive](https://drive.google.com/drive/folders/11BSDjUA4fb9uLnAqSFXjKZWQbzVadSIn?usp=sharing).
- **Script:** [`hicdcplus_run.R`](https://github.com/viannegao/ChromaFold/blob/main/process%20input/hic_normalization/hicdcplus_run.R)

```
Rscript /chromafold/process input/hic_normalization/hicdcplus_run.R \
10000 \
"/scripts/hicdc_workflow/hg38_MboI_10kb_features.rds" \
"/data/HiC/imr90/hicdc_out/" \
"Hsapiens" \
"hg38" \
"/data/HiC/imr90/ENCFF281ILS.hic" \
"scripts/hicdc_workflow/straw.cpp" \
"/scripts/hicdc_workflow/juicer_tools.1.9.9_jcuda.0.8.jar" 
```

After this step, HiC-DC+ normalized files are successfully generated, including 
- `.rds`: R object, stores all the normalization calculations
- `.hic`: z-score and q-value from the normalization have been stored into `.hic` format. 

### **Step 2**. Extract `.hic` values to `.txt`

```
for i in {1..22}
do

java -jar  /scripts/hicdc_workflow/juicer_tools_1.22.01.jar dump observed NONE /data/HiC/imr90/ENCFF281ILS_zvalue.hic $i $i BP 10000 /data/HiC/imr90/hicdc_out/zvalue/chr"$i"_raw.txt

java -jar  /scripts/hicdc_workflow/juicer_tools_1.22.01.jar dump observed NONE /data/HiC/imr90/ENCFF281ILS_qvalue.hic $i $i BP 10000 /data/HiC/imr90/hicdc_out/qvalue/chr"$i"_raw.txt
echo $i

done
```

These `.txt` files are used in the data integration step in the [notebook](https://github.com/viannegao/ChromaFold/blob/main/process%20input/Process%20Input%20-%20Hi-C.ipynb).