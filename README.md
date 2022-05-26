# Cut-RunPeakAnalysis_Snakemake


## Step 1 - Downsample .bam file

```bash
reads=10000000
bam=your.bam
fraction=$(samtools idxstats $bam | cut -f3 | awk -v ct=$reads 'BEGIN {total=0} {total += $1} END {print ct/total}')
samtools view -b -s ${fraction} foo.bam > sampled.bam
```

## Step 2 - Run computeMatrix

```bash
computeMatrix scale-regions -S H3K27Me3-input.bigWig \
                                 H3K4Me1-Input.bigWig  \
                                 H3K4Me3-Input.bigWig \
                              -R genes19.bed genesX.bed \
                              --beforeRegionStartLength 3000 \
                              --regionBodyLength 5000 \
                              --afterRegionStartLength 3000
                              --skipZeros -o matrix.mat.gz
```

## Step 3 - Run plotProfile

```bash
plotProfile -m matrix.mat.gz \
              -out ExampleProfile1.png \
              --numPlotsPerRow 2 \
              --plotTitle "Test data profile"
```
