<p align="center">
  <img src="docs/kids_first_logo.svg" alt="Kids First repository logo" width="660px" />
</p>
<p align="center">
  <a href="https://github.com/kids-first/kf-mskcc-vcf2maf/blob/master/LICENSE"><img src="https://img.shields.io/github/license/kids-first/kf-template-repo.svg?style=for-the-badge"></a>
</p>

# Kids First Port of MSKCC's vcf2maf

This is a port of MSKCCs [vcf2maf](https://github.com/mskcc/vcf2maf) repository, release v1.16.19.
We have modified their `Dockerfile` and dropped deprecated fields from their maf output to suit the needs of Kids First and
pedcBioportal


### Repo Description

This port of MSKCCs vcf2maf is a bit more minimal in function in that it is expected that VEP would be run seperately, using recomended commands. For example:

```
perl /ensembl-vep-release-93.7/vep
      --af
      --af_1kg
      --af_esp
      --af_gnomad
      --allele_number
      --assembly $(inputs.ref_build)
      --biotype
      --buffer_size 10000
      --cache
      --cache_version $(inputs.cache_version)
      --canonical
      --ccds
      --check_existing
      --dir_cache $(inputs.species)
      --domains
      --failed 1
      --fasta $(inputs.reference.path)
      --flag_pick_allele
      --fork 16
      --format vcf
      --gene_phenotype
      --hgvs
      --input_file $(inputs.input_vcf.path)
      --no_escape
      --no_progress
      --no_stats
      ${
        if(inputs.merged){
          return "--merged";
        }
        else{
          return "";
        }
      }
      --numbers
      --offline
      --output_file $(inputs.output_basename).$(inputs.tool_name).PASS.vep.vcf
      --pick_order canonical,tsl,biotype,rank,ccds,length
      --polyphen b
      --protein
      --pubmed
      --regulatory
      --shift_hgvs 1
      --sift b
      --species $(inputs.species)
      --symbol
      --total_length
      --tsl
      --uniprot
      --variant_class
      --vcf
```

Typical usage of the `vcf2maf.pl` script, within the built docker:
```
perl /opt/vcf2maf.pl --input-vcf <input_vcf> --output-maf <output_maf file name> --tumor-id <tumor sample ID> --normal-id <normal sample ID> --custom-enst /opt/data/isoform_overrides_uniprot --ncbi-build GRCh38 --inhibit-vep --retain-info MQ,MQ0,CAL,Hotspot --ref-fasta Homo_sapiens_assembly38.fasta
```