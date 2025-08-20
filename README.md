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
    * GRCh38_GIABv3_no_alt_analysis_set_maskedGRC_decoys_MAP2K3_KMT2C_KCNJ18_noChr.fasta-index.tar.gz
* Custom Annotation sources:
    * ClinVar
        * clinvar_20250810_GRCh38.vcf.gz
    * gnomADe
        *   gnomad.exomes.v4.1.sites.all.trimmed_normalised_decomposed_PASS.no_chr.vcf.bgz
    * gnomADg
        *   gnomad.genomes.v4.1.sites.all.trimmed_normalised_decomposed_PASS.no_chr.vcf.bgz
    * COSMIC
        * CosmicCodingMuts_GRCh38_v100.normal.vcf.gz
        * CosmicNonCodingVariants_GRCh38_v100.normal.vcf.gz
    * uranus_panel_v4_annotation_for_vep.bed.gz
    * haemonc_1706_samples_withoutchr.vcf.gz
    * uranus_variants_rescue_list_nochr_v1.1.sorted.bed.gz
    * haemonc_annotation_VCF_20250224_102322.vcf.gz
* Plugin annotations:
    * SpliceAI
        * spliceai_scores.masked.snv.hg38.vcf.gz
        * spliceai_scores.masked.indel.hg38.vcf.gz
    * CADD
        * whole_genome_SNVs.tsv.gz
        * gnomad.genomes.r3.0.indel.tsv.gz

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