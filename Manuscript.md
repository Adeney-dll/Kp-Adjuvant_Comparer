## In silico design of a multi-epitope vaccine candidate against *Klebsiella pneumoniae* targeting the outer membrane lipoprotein Pal

Fatuyi Adene Adeniyi · Computational Biology Researcher · [27 / 10 / 2026]

<!-- This file holds the full methods, numbers and references. The README carries only the headline results and images. -->

---

#### Abstract
*Klebsiella pneumoniae* remains one of world's most significant organisms, it's high pathogenicity inducing numerous diseases, such as liver abscesses, bacteremias, pneumonia, and urinary tract infections, which constitute a major threat to hospitals catering to the immunocompromised and the community. No licensed vaccine is available.
Using a reverse-vaccinology workflow, built from free web servers and short Python filtering scripts, to design a multi-epitope vaccine against the outer membrane lipoprotein Pal. A consensus of ten Pal sequences was confirmed as an outer membrane lipoprotein (PSORTb outer membrane score 10.00; SignalP 6.0 Sec/SPII signal peptide, cleavage after residue 21). B-cell, MHC-I and MHC-II epitopes were predicted and filtered for antigenicity, toxicity and allergenicity (and immunogenicity for MHC-II). Two constructs were assembled, one with cholera toxin B subunit (MEV-1) and one with human β-defensin 2 (MEV-2) as adjuvant, each with a thioredoxin tag.
One B-cell and five MHC-II epitopes passed all filters; none of the 37 MHC-I candidates did. Both constructs had instability indices below 40 (32.29 and 31.75) and more than 91% of residues in favoured Ramachandran regions. Docking to TLR4 gave binding free energies of −24.05 kcal/mol (MEV-1) and −51.36 kcal/mol (MEV-2). Codon optimisation raised GC content from 31.1% to 49.9% and from 33.3% to 52.5%, and both genes were cloned virtually into pET-28a(+). Immune simulation predicted a substantial rise in memory response.
Both constructs meet the computational criteria applied here and are candidates for further testing. All results are predictions; no laboratory validation was done.

---

#### Introduction
_Klebsiella pneumoniae_ is a Gram-negative bacterium that causes pneumonia, urinary tract infections and bloodstream infections, mainly in hospitalised and immunocompromised patients **[1]**. Hypervirulent strains add invasive community-acquired disease **[2]**. Its virulence depends on the capsule, lipopolysaccharide, fimbriae and siderophores **[1–2]**. As treatment options narrow, prevention becomes more attractive, yet no licensed vaccine exists.

Capsule and lipopolysaccharide vary widely between isolates, which makes them difficult vaccine targets. Outer membrane proteins are generally more conserved. The Pal protein is one of these: a lipoprotein anchored in the outer membrane that links it to the peptidoglycan layer. Other outer membrane proteins were assessed such as: OmpA and LppA, the Pal protein was settled on due to sparse literature on reverse vaccinology on this target **[3]**.

Multi-epitope vaccines join selected B-cell and T-cell epitopes with short linkers and an adjuvant, giving a small, defined antigen that can be produced in _E. coli_. Computational pipelines allow many candidates to be screened before any laboratory work. Here such pipeline was applied to Pal, two constructs were designed that differ in adjuvant, and evaluated for stability, structure, TLR4 binding, expression design and simulated immune response. The work is computational only and does not test the vaccines experimentally.


#### Methods
Table 1. Tools and settings

| Step                     | Tool             | Version | Settings                                                      | Ref       |
| ------------------------ | ---------------- | ------- | ------------------------------------------------------------- | --------- |
| Sequence retrieval       | NCBI Protein     |         | Klebsiella pneumoniae Pal ATCC 13883"; 170–185 aa; n = 10     | [4]       |
| Alignment                | MEGA 12 (MUSCLE) | 12      | defaults                                                      | [5] [6]   |
| Localisation             | PSORTb           | 3.0.3   | Gram-negative                                                 | [7]       |
| Signal peptide           | SignalP          | 6.0     | Sec/SPII                                                      | [8]       |
| B-cell epitopes          | IEDB BepiPred    | 3.0     | defaults                                                      | [9]       |
| MHC-I                    | NetMHCpan EL     | 4.1     | 27 alleles                                                    | [10]      |
| MHC-II                   | NetMHCIIpan EL   | 4.1     | 27 alleles                                                    | [10] [11] |
| Antigenicity             | VaxiJen          | 2.1     | threshold 0.3                                                 | [12]      |
| Toxicity                 | ToxinPred        | 3.0     | defaults                                                      | [13]      |
| Allergenicity            | AlgPred          | 2.0     | defaults                                                      | [14]      |
| Immunogenicity           | Vaxijen          | 2.0     | defaults                                                      | [15]      |
| 3D structure             | AlphaFold        | 3       |                                                               | [16]      |
| Physicochemical Property | ExPASy ProtParam |         |                                                               | [17]      |
| Secondary structure      | SOPMA            |         |                                                               | [18]      |
| Structure assessment     | SWISS-MODEL      |         |                                                               | [19]      |
| Docking                  | HawkDock         | 2       | TLR4 chain A, PDB 4G8A                                        | [20] [21] |
| Flexibility              | WEBnm@           | 2.0     |                                                               | [22]      |
| Codon optimisation       | DNA Chisel       |         | E. coli; GC 40–65%; max homopolymer 7 nt                      | [23]      |
| In silico cloning        | Benchling        |         | pET-28a(+), NdeI/XhoI                                         | [24]      |
| Immune simulation        | C-ImmSim         |         | volume 20; seed 12345; 1,000 steps; doses at steps 1, 84, 168 | [25] [26] |

##### Target selection and sequence retrieval

Three outer membrane proteins (OmpA, LppA and Pal) were shortlisted from published work on _K. pneumoniae_ [1–3]. Pal was selected based on sparse findings on its use in reverse vaccinology. Pal protein sequences were retrieved from the NCBI Protein database [4] on [4/09/2026] using the query “Klebsiella pneumoniae Pal ATCC 13883”, restricted to 170–185 amino acids, and ten sequences were kept (accessions in `data/raw_sequences`).


##### Consensus sequence, localisation and signal peptide

The ten sequences were aligned with MUSCLE in MEGA12 [5-6] using default settings, and a consensus was derived. Subcellular localisation was predicted with PSORTb [7], and signal peptides with SignalP 6.0 **[Sec/SPII]** [8]. The signal peptide was removed to give the mature sequence used for epitope prediction.


##### Epitope prediction

Linear B-cell epitopes were predicted with BepiPred 3.0 [9]. MHC-I and MHC-II binding peptides were predicted with NetMHCpan EL 4.1 [10] and NetMHCIIpan EL 4.1 [10-11] against a panel of 27 human alleles provided on the platform, using peptide lengths of **[8–11 / 15]**.


##### Epitope filtering

Epitopes shorter than six residues were discarded. Top binders were selected by **[rank / percentile cut-off]** with a Python script (`scripts/filter_scripts`). Retained epitopes were screened for antigenicity with VaxiJen 2.1 at a threshold of **[0.3]** [12], toxicity with ToxinPred 3.0 [13], and allergenicity with AlgPred 2.0 [14]. MHC-II epitopes were also screened for immunogenicity with Vaxijen 2.0 [15]. Epitopes had to pass all criteria to be kept.


##### Construct design

Retained epitopes were joined with GPGPG linkers, and the epitope block was joined to the adjuvant by an EAAAK linker. Cholera toxin B subunit (MEV-1) or human β-defensin 2 (MEV-2) (accessions in `data/raw_sequences/adjuvants`). were used as adjuvants, with signal peptides removed. Both constructs were unstable at first (instability index **[>40]**), so a 109-residue thioredoxin tag [27] was added at the N-terminus, and a 6xHis-tag was placed at the C-terminus. Overlapping epitopes were handled by merging.


##### Physicochemical properties, structure prediction and validation

Properties were calculated with ExPASy ProtParam [17]. Secondary structure was predicted with SOPMA [18] and tertiary structure with AlphaFold**3** [16]. Models were assessed in SWISS-MODEL [19] by Ramachandran favoured percentage, MolProbity score, clashscore and QMEAN / QMEANDisCo.


##### Molecular docking

Each construct was docked to human TLR4 (PDB 4G8A, chain A) using HawkDock [20-21]. Binding free energy was estimated by MM/GBSA, and interface residues were listed from the top-ranked model.


##### Flexibility analysis

Normal mode analysis was run in WEBnm@ [22] on the top model of each construct, and deformation energy and fluctuation plots were recorded.

##### Codon optimisation and in silico cloning

The back-translated sequences were codon-optimised for _E. coli_ K-12 with DNA Chisel [23], keeping GC content between 40% and 65% and avoiding homopolymer runs longer than 7 nt and sites for **[_XhoI_, _EcoRI_, _BamHI_, _HindIII_]**. NdeI and XhoI sites were added at the ends, and the inserts were cloned into pET-28a(+) in Benchling [24]. Insert length, GC content and digest fragments were recorded.

##### Immune simulation

Immune responses were simulated with C-ImmSim [25=26] using a volume of 20, random seed 12345, and 1,000 steps, with three doses at steps 1, 84 and 168 (days 0, 28 and 56). Default HLA alleles (HLA-A_02:01, HLA-B_07:02, HLA-DRB1*01:01) were used. The input sequence was *with Trx and His-tag*.


#### Results
##### Target sequence and mature protein

Ten Pal sequences (170–185 aa) gave a 174-residue consensus (Fig. 3A). PSORTb assigned it to the outer membrane with a score of 10.00. SignalP 6.0 predicted a lipoprotein (Sec/SPII) signal peptide with cleavage between residues 21 and 22 (probability **[0.993814]**; Fig. 3B), leaving a 153-residue mature sequence.

<img src="results/raw_images/MEGA_ 01.png" alt="MEGA" width="800">

Figure 3A: MEGA Alignment screen with the top consensus sequence being highlighted

Table 2: PSORTb Localisation scores

| Cytoplasmic          | 0.00  |
| -------------------- | ----- |
| Cytoplasmic Membrane | 0.00  |
| Periplasmic          | 0.00  |
| Outer Membrane       | 10.00 |
| Extracellular        | 0.00  |

<img src="results/SignalP output_Consesus_plot.png" alt="MEGA" width="800">
Figure 3B: SignalP plot showing signal peptide location along the protein


##### Epitope prediction and filtering

BepiPred 3.0 predicted **[4]** B-cell epitopes, and the two NetMHC tools returned **[3915]** MHC-I and 756 MHC-II top binders. After filtering, one B-cell epitope, five MHC-II epitopes and no MHC-I epitopes remained (Table 3a-b).

Table 3A:  Passing B-cell epitope

| Peptide                                    | Antigenicity      | Toxicity  | Allergenicity         |
| ------------------------------------------ | ----------------- | --------- | --------------------- |
| NKNASNDQSGEGMLGAGTGMDANGNGGNMSSEEQARLQMQQL | Probable antigen. | Non-Toxin | Probable Non-Allergen |


Table 3B: Passing MHC-II epitopes

| Peptide         | Antigenicity | Toxicity  | Allergenicity | Immunogenicity |
| --------------- | ------------ | --------- | ------------- | -------------- |
| DKYDIRSDFAAMLDA | antigen      | Non-Toxin | Non-Allergen  | Immunogen      |
| LGERRANAVKMYLQG | antigen      | Non-Toxin | Non-Allergen  | Immunogen      |
| NGNGGNMSSEEQARL | antigen      | Non-Toxin | Non-Allergen  | Immunogen      |
| AMLDAHANFLRSNPS | antigen      | Non-Toxin | Non-Allergen  | Immunogen      |
| VYFDLDKYDIRSDFA | antigen      | Non-Toxin | Non-Allergen  | Immunogen      |


##### Construct properties

MEV-1 (320 residues, **[34.9]** kDa) and MEV-2 (258 residues, **[27.6]** kDa) had instability indices of 32.29 and 31.75 after addition of the Trx tag, below the threshold of 40. VaxiJen antigenicity scores were 0.557 and 0.636, and both were predicted non-toxic and non-allergenic. SOPMA predicted **[57.19%]** helix, **[8.44%]** extended strand and **[29.06%]** coil for MEV-1 and **[45.74%, 10.08%, 36.78%]** for MEV-2. AlphaFold models are shown in Fig. 5(a-b).

Table 4: Construct physiochemical properties

| Physiochemical Properties               | MEV-1                  | MEV-2                  |
| --------------------------------------- | ---------------------- | ---------------------- |
| Total Number of Amino Acids             | 320                    | 258                    |
| Molecular Weight (MW)                   | 34902.51 Da / 34.9 kDa | 27591.39 Da / 27.6 kDa |
| Theoretical pI (Isoelectric Point)      | 5.88                   | 6.49                   |
| Instability Index (II)                  | 32.29                  | 31.75                  |
| Aliphatic Index                         | 79.72                  | 74.61                  |
| GRAVY (Grand Average of Hydropathicity) | -0.386                 | -0.367                 |

<img src="results/Structures/AlphaFold/MEVs/MEV-1-Alp.png" alt="mev" width="600">

Figure 5a: AlphaFold Tertiary structure prediction for MEV-1

<img src="results/Structures/AlphaFold/MEVs/MEV-2-Alp.png" alt="mev" width="600">

Figure 5b: AlphaFold Tertiary structure prediction for MEV-2


##### Structural validation

Ramachandran favoured values were 92.45% (MEV-1) and 91.02% (MEV-2), both above 90%. MolProbity scores were 1.95 and 2.00 and clashscores 4.95 and 3.93. QMEANDisCo global scores were 0.58 and 0.53 and the QMEAN Z-score for MEV-1 was −1.96 (Table 5).

Table 5: MEV structural validation

| Parameter               | MEV-1       | MEV-2       |
| ----------------------- | ----------- | ----------- |
| Ramachandran Favoured   | 92.45%      | 91.02%      |
| MolProbity Score        | 1.95        | 2.00        |
| Clash Score             | 4.95        | 3.93        |
| QMEANDisCo Global Score | 0.58 ± 0.05 | 0.53 ± 0.05 |
| QMEAN Z-Score           | -1.96       | -1.20       |

##### Docking with TLR4

MEV-1 and MEV-2 formed complexes with TLR4 (Fig. 6) with binding free energies of −24.05 and −51.36 kcal/mol (Table 6).

Table 6: Consolidated interaction table

| Construct | Receptor          | Docking Score | TLR4 (Chain A) Interface Residues                                                                                                                                                     | MEV Interface Residues                                                                                                 |
| --------- | ----------------- | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| MEV-1     | TLR4, (PDB: 4G8A) | -5323.90      | Lys21 Phe28 Ser45 Tyr46 Phe49 Pro52 Glu68 His203 His230 Arg231 Glu260 Glu261 Tyr377 Glu399 His400 Ile424 Glu448                                                                       | Ala88 Ala89 Thr90 Glu102 Ala106 Asn107 Gln112 Pro249 Gly250 Pro251 Gly252 Glu255 Gln266 Pro269 Gly270                  |
| MEV-2     | TLR4, (PDB: 4G8A) | -3867.47      | Arg238 Glu244 Tyr266 Leu267 Asp268 Tyr269 Tyr270 Thr293 Lys315 Lys336 Gly337 Asn339 Asn383 Gly384 Asn407 His432 Arg434 Gly454 Asn455 Ser456 Phe457 Glu459 Asn460 Gln481 Glu483 Gln484 | Met239 Ser240 Ser241 Glu242 Glu243 Gln244 Ala245 Leu247 Gln248 Gln250 Gln251 Leu252 His253 His254 His255 His256 His257 |

<img src="results/Structures/Docking/complex_docking_MEV1.png" alt="mev" width="700">
Figure 6a: MEV-1[TLR4] complex [MEV structure in purple and TLR4 fragment in cyan]

<img src="results/Structures/Docking/complex_docking_MEV2.png" alt="mev" width="700">
Figure 6b: MEV-2[TLR4] complex


##### Flexibility, codon optimisation and cloning

 Normal mode analysis eigenvalue / deformation energy range for MEV-1 and MEV-2 (Fig. 7), indicating. Codon optimisation raised GC content from 31.1% to 49.9% (MEV-1) and from 33.3% to 52.5% (MEV-2). Inserts of 972 and 786 bp (after adding NdeI and XhoI) were cloned into the 5,289-bp pET-28a(+) backbone (Fig. 8, Table 7).

<img src="results/Normal-mode-analysis/MEV-1/MEV-1I.jpg" alt="mev" width="700">
Figure 7a: The correlation, deformation, displacement, and eigen plots for MEV-1

<img src="results/Normal-mode-analysis/MEV-2/MEV-2I.jpg" alt="mev" width="700">

Figure 7b: The correlation, deformation, displacement, and eigen plots for MEV-2


Table 7: Codon optimization

|                                  | MEV-1             | MEV-2             |
| -------------------------------- | ----------------- | ----------------- |
| Sequence Length                  | 960 nt (bp)       | 774 nt (bp)       |
| GC Content (Before Optimization) | 31.1%             | 33.3%             |
| GC Content (After Optimization)  | 49.9%             | 52.5%             |
| Codon Modifications              | 186 of 320 codons | 153 of 258 codons |


<img src="results/Cloning/pET-28a_MEV-1.png" alt="mev" width="700">

<img src="results/Cloning/pET-28a_MEV-2.png" alt="mev" width="700">


Figure 8: Cloning was achieved using the pET-28a(+) expression vector utilizing the NdeI and XhoI restriction sites.


##### Simulated immune response

Across three doses, C-ImmSim predicted **[antibody titres: IgM / IgG levels and peak day]**, **[memory B-cell and T-helper trends]**, **[cytotoxic T-cell response]**, and **[cytokine levels]** for both constructs (Fig. 9).

<img src="results/Immune-Simulation/MEV-1/IM-CM.png" alt="mev" width="700">



3. Discussion
We designed two Pal-based multi-epitope constructs and compared them across the same checks. MEV-1 scored slightly better on structure (92.45% against 91.02% favoured Ramachandran residues; MolProbity 1.95 against 2.00), while MEV-2 showed stronger predicted TLR4 binding (−51.36 against −24.05 kcal/mol) and a higher antigenicity score (0.636 against 0.557). **[State which you take forward and why.]** Several structural values sit close to their cut-offs, so both constructs are better described as within accepted ranges than as optimal.

Only six epitopes survived filtering, and none were MHC-I. The constructs therefore rely on helper T-cell and B-cell epitopes, which limits how much a cytotoxic response can be expected to contribute. **[Say which filter removed the MHC-I candidates, and whether the simulated cytotoxic T-cell response matches this.]**

Both constructs were unstable until a 109-residue thioredoxin tag was added [15]. The tag fixed the instability index, but it makes up about 34% of MEV-1 (109 of 320 residues) and 42% of MEV-2 (109 of 258), and none of it comes from _Klebsiella_. **[State whether Trx is meant to stay in the final vaccine or be cleaved.]** If it stays, its effect on the immune response and on cross-reactivity with the human thioredoxin homologue needs checking.

The docking results should be read cautiously. HawkDock energies are model estimates, not measured affinities, and the receptor was a TLR4 monomer without MD-2. **[If confirmed: several contact residues lie in the GPGPG linker and the His-tag rather than the epitopes or adjuvant, so the predicted binding may not reflect the designed antigenic regions.]** Docking the adjuvants alone would give a baseline to compare against.

Next steps are a BLASTp screen of the six epitopes against the human proteome, a check of conservation across more _K. pneumoniae_ genomes, population coverage analysis, molecular dynamics of the top construct, and expression tests in _E. coli_.


3. Limitations
Computational only; no expression, safety or immunogenicity data.
Prediction tools depend on their training data; several score thresholds are conventions, not proofs.
Docking used a TLR4 monomer without MD-2; no reference docking for comparison.
C-ImmSim used default HLA alleles and a generic host.
4. Conclusion


#### References
[1] Abbas, R., Chakkour, M., Dine, H. Z. E., Obaseki, E. F., Obeid, S. T., Jezzini, A., Ghssein, G., & Ezzeddine, Z. (2024). General Overview of Klebsiella pneumonia: Epidemiology and the Role of Siderophores in Its Pathogenicity. Biology, 13(2), 78. https://doi.org/10.3390/biology13020078

[2] Zhu, J., Wang, T., Chen, L., & Du, H. (2021). Virulence Factors in Hypervirulent Klebsiella pneumoniae. _Frontiers in Microbiology_, _12_, 642484. https://doi.org/10.3389/fmicb.2021.642484

[3] Paczosa, M. K., & Mecsas, J. (2016). Klebsiella pneumoniae: Going on the Offense with a Strong Defense. _Microbiology and Molecular Biology Reviews_, _80_(3), 629–661. https://doi.org/10.1128/mmbr.00078-15

[4] Sayers, E. W., Bolton, E. E., Fine, A. M., Kelly, C., Kim, S., Landrum, M., Lathrop, S., Malheiro, A., Murphy, T. D., Phan, L., Pujar, S., Trawick, B. W., Schneider, V. A., & Pruitt, K. D. (2025). Database resources of the National Center for Biotechnology Information in 2026. _Nucleic Acids Research_, _54_(D1), D20–D27. https://doi.org/10.1093/nar/gkaf1060

[5] Kumar, S., Stecher, G., Suleski, M., Sanderford, M., Sharma, S., & Tamura, K. (2024). MEGA12: Molecular Evolutionary Genetic Analysis Version 12 for Adaptive and Green Computing. _Molecular Biology and Evolution_, _41_(12). https://doi.org/10.1093/molbev/msae263

[6] Edgar, R. C. (2004). MUSCLE: multiple sequence alignment with high accuracy and high throughput. _Nucleic Acids Research_, _32_(5), 1792–1797. https://doi.org/10.1093/nar/gkh340

[7] Yu, N. Y., Wagner, J. R., Laird, M. R., Melli, G., Rey, S., Lo, R., Dao, P., Sahinalp, S. C., Ester, M., Foster, L. J., & Brinkman, F. S. L. (2010). PSORTb 3.0: improved protein subcellular localization prediction with refined localization subcategories and predictive capabilities for all prokaryotes. _Bioinformatics_, _26_(13), 1608–1615. https://doi.org/10.1093/bioinformatics/btq249

[8] Nielsen, H., Teufel, F., Brunak, S., & Von Heijne, G. (2024). SignalP: The Evolution of a Web Server. In _Methods in molecular biology_ (Vol. 2836, pp. 331–367). https://doi.org/10.1007/978-1-0716-4007-4_17

[9] Yan, Z., Kim, K., Kim, H., Ha, B., Gambiez, A., Bennett, J., De Almeida Mendes, M. F., Trevizani, R., Mahita, J., Richardson, E., Marrama, D., Blazeska, N., Koşaloğlu-Yalçın, Z., Nielsen, M., Sette, A., Peters, B., & Greenbaum, J. A. (2024). Next-generation IEDB tools: a platform for epitope prediction and analysis. _Nucleic Acids Research_, _52_(W1), W526–W532. https://doi.org/10.1093/nar/gkae407

[10] Reynisson, B., Alvarez, B., Paul, S., Peters, B., & Nielsen, M. (2020). NetMHCpan-4.1 and NetMHCIIpan-4.0: improved predictions of MHC antigen presentation by concurrent motif deconvolution and integration of MS MHC eluted ligand data. _Nucleic Acids Research_, _48_(W1), W449–W454. https://doi.org/10.1093/nar/gkaa379

[11] Kaabinejadian, S., Barra, C., Alvarez, B., Yari, H., Hildebrand, W. H., & Nielsen, M. (2022). Accurate MHC motif deconvolution of immunopeptidomics data reveals a significant contribution of DRB3, 4 and 5 to the total DR immunopeptidome. _Frontiers in Immunology_, _13_, 835454. https://doi.org/10.3389/fimmu.2022.835454

[12] Doytchinova, I. A., & Flower, D. R. (2007). VaxiJen: a server for prediction of protective antigens, tumour antigens and subunit vaccines. _BMC Bioinformatics_, _8_(1), 4. https://doi.org/10.1186/1471-2105-8-4

[13] Rathore, A. S., Choudhury, S., Arora, A., Tijare, P., & Raghava, G. P. (2024). ToxinPred 3.0: An improved method for predicting the toxicity of peptides. _Computers in Biology and Medicine_, _179_, 108926. https://doi.org/10.1016/j.compbiomed.2024.108926

[14] Sharma, N., Patiyal, S., Dhall, A., Pande, A., Arora, C., & Raghava, G. P. S. (2020). AlgPred 2.0: an improved method for predicting allergenic proteins and mapping of IgE epitopes. _Briefings in Bioinformatics_, _22_(4). https://doi.org/10.1093/bib/bbaa294

[15] Dimitrov, I., Zaharieva, N., & Doytchinova, I. (2020). Bacterial immunogenicity prediction by machine learning methods. _Vaccines_, _8_(4), 709. https://doi.org/10.3390/vaccines8040709

[16] Abramson, J., Adler, J., Dunger, J., Evans, R., Green, T., Pritzel, A., Ronneberger, O., Willmore, L., Ballard, A. J., Bambrick, J., Bodenstein, S. W., Evans, D. A., Hung, C., O’Neill, M., Reiman, D., Tunyasuvunakool, K., Wu, Z., Žemgulytė, A., Arvaniti, E., . . . Jumper, J. M. (2024). Accurate structure prediction of biomolecular interactions with AlphaFold 3. _Nature_, _630_(8016), 493–500. https://doi.org/10.1038/s41586-024-07487-w

[17] Gasteiger, E., Hoogland, C., Gattiker, A., Duvaud, S., Wilkins, M. R., Appel, R. D., & Bairoch, A. (2005). Protein identification and analysis tools on the ExPASY server. In _Humana Press eBooks_ (pp. 571–607). https://doi.org/10.1385/1-59259-890-0:571

[18] Geourjon, C., & Deléage, G. (1995). SOPMA: significant improvements in protein secondary structure prediction by consensus prediction from multiple alignments. _Computer Applications in the Biosciences_, _11_(6), 681–684. https://doi.org/10.1093/bioinformatics/11.6.681

[19] Waterhouse, A. M., Studer, G., Robin, X., Bienert, S., Tauriello, G., & Schwede, T. (2024). The structure assessment web server: for proteins, complexes and more. _Nucleic Acids Research_, _52_(W1), W318–W323. https://doi.org/10.1093/nar/gkae270

[20] Berman, H. M. (2000). The Protein Data Bank. _Nucleic Acids Research_, _28_(1), 235–242. https://doi.org/10.1093/nar/28.1.235

[21] Zhang, X., Jiang, L., Weng, G., Shen, C., Zhang, O., Liu, M., Zhang, C., Gu, S., Wang, J., Wang, X., Du, H., Zhang, H., Zhang, K., Wang, E., & Hou, T. (2025). HawkDock version 2: an updated web server to predict and analyze the structures of protein–protein complexes. _Nucleic Acids Research_, _53_(W1), W306–W315. https://doi.org/10.1093/nar/gkaf379

[22] Tiwari, S. P., Fuglebakk, E., Hollup, S. M., Skjærven, L., Cragnolini, T., Grindhaug, S. H., Tekle, K. M., & Reuter, N. (2014). WEBnm@ v2.0: Web server and services for comparing protein flexibility. _BMC Bioinformatics_, _15_(1), 427. https://doi.org/10.1186/s12859-014-0427-6

[23] Zulkower, V., & Rosser, S. (2020). DNA Chisel, a versatile sequence optimizer. _Bioinformatics_, _36_(16), 4508–4509. https://doi.org/10.1093/bioinformatics/btaa558

[24] Benchling. (2026).

[25] Rapin, N., Lund, O., & Castiglione, F. (2011). Immune system simulation online. _Bioinformatics_, _27_(14), 2013–2014. https://doi.org/10.1093/bioinformatics/btr335

[26] Basak, S., Deb, D., Narsaria, U., Kar, T., Castiglione, F., Sanyal, I., Bade, P. D., & Srivastava, A. P. (2021). In silico designing of vaccine candidate against Clostridium difficile. _Scientific Reports_, _11_(1), 14215. https://doi.org/10.1038/s41598-021-93305-6

[27] LaVallie, E. R., DiBlasio, E. A., Kovacic, S., Grant, K. L., Schendel, P. F., & McCoy, J. M. (1993). A Thioredoxin Gene Fusion Expression System That Circumvents Inclusion Body Formation in the E. coli Cytoplasm. _Nature Biotechnology_, _11_(2), 187–193. https://doi.org/10.1038/nbt0293-187


