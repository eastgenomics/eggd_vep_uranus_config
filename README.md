# eggd_vep_uranus_config

This repo contains a JSON configuration file for the implementation of VEP for the HaemOnc assay.

## What does the JSON config do?
Configuration file required to annotate a vcf using Variant Effect Predictor implementation eggd_vep.

A variable level of annotation can be achieved by different combinations of custom annotations and vep plugins, in addition to the required VEP resources.

## What does the JSON config version contain?
This json file provides information about annotations,plugins, required fields and the genome version.

* Genome build: GRCh38
* VEP required files:
    * vep_v103.1_docker.tar.gz
    * homo_sapiens_refseq_vep_103_GRCh38.tar.gz
    * plugin_config.txt
    * Homo_sapiens.GRCh38.dna.toplevel.fa.gz
    * Homo_sapiens.GRCh38.dna.toplevel.fa.gz.fai
    * Homo_sapiens.GRCh38.dna.toplevel.fa.gz.gzi
    * GRCh38.no_alt_analysis_set_chr_mask21.fasta-index.tar.gz
* Custom Annotation sources:
    * ClinVar
        * clinvar_20231217_hg38_withchr.vcf.gz
    * COSMIC
        * CosmicCodingMuts_GRCh38_v94.normal.vcf.gz
        * CosmicNonCodingVariants_GRCh38_v94.normal.vcf.gz
    * uranus_v2_panel_sorted_int.bed.gz
    * haemonc_1706_samples.vcf.gz
* Plugin annotations:
    * CADD
        * whole_genome_SNVs.tsv.gz
        * gnomad.genomes.r3.0.indel.tsv.gz
* VEP Annotation
    * gnomad_AF (exome)

Notes
How to check the names of all the files included in the config:

```bash
config_file=you_file_name.json

# Get the Vep Resources filenames
for file in  $(jq -r ' .vep_resources | .[]' $config_file);
do dx describe $file --json | jq -r '.name';
done

# Get Custom Annotation filenames
for file in  $(jq -r ' .custom_annotations[]|.resource_files[]|.file_id' $config_file);
do dx describe $file --json | jq -r '.name';
done

# Get Plugin Annotation filenames
for file in  $(jq -r ' .plugins[]|.resource_files[]|.file_id' $config_file);
do dx describe $file --json | jq -r '.name';
done
```