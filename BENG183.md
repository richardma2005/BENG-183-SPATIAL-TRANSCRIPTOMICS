<div align="center">

# Presentation 15: Spatial Transcriptomics  
### 11/25/2025; Alex Mueller, Richard Ma, Weber Lai  

</div>

### Introduction to Spatial Transcriptomics

Sequencing technologies offer vitally important insight into the structure and composition of the transcriptome and genome: RNA sequencing, for example, allows us to identify differentially expressed genes and pinpoint cell differentiation, while HI-C technologies offer insight into the 3D structure of the genome. Yet, in these cases, spatial information is lost: there is no direct mapping between the sequencing data and the sample. This lack of information creates a fundamental disconnect between molecular identity and tissue structure, especially in tissues where physical arrangement is important. This limitation is addressed directly by spatial transcriptomic methods.

Spatial transcriptomics works to simultaneously preserve spatial information and report gene expression in a sample. It analyzes the transcriptome (the transcribed RNAs and, by extension, expressed genes) of a tissue sample in space; it divides the tissue sample into specific regions of interest (ROIs), allowing us to measure each region’s expression, even on the scale of single cells.[^4]

Indeed, unlike other sequencing technologies, the tissue’s structure is preserved, allowing for the mapping of cell-to-cell interactions and spatial categorization of cells. Therefore, these techniques prove particularly useful in studying cell differentiation and communication (by the detection of surface receptor mRNAs, for instance).[^7] Tissue spatial information further allows for the study of disease and immune response. By mapping mouse brains infected with Alzheimer's, for example, researchers were able to pinpoint the particular expression patterns with cells neighboring beta-amyloid plaques; similar trends of plaque-induced differential expression were further observed in humans, notably in immune genes.[^6][^7] Ultimately, spatial transcriptomics proves powerful in elucidating patterns of expression within tissues and how expression changes locally within them in response to conditions such as disease. 

Varying between technologies, approaches for spatial transcriptomics largely fall into image-based and sequencing-based techniques.[^4] Image-based techniques (ex: MERFISH) rely on the binding of fluorescently labeled targeted probes to transcripts of interest, followed by repeated cycles of fluorescence imaging. This allows researchers to directly image a sample and elucidate expression by recording where each probe binds in the tissue.[^3][^4] However, the number of genes that can be fluorescently observed is limited by the number of gene panels (the specific targeted probes used).[^3][^4] Sequencing-based methods, on the other hand, rely on next-generation sequencing (NGS) and directly capture and sequence mRNA from the sample, eliminating the need for specifically targeted probes. NGS methods are unbiased and can detect novel mRNAs, as they do not target specific mRNA transcripts. NGS methods provide higher throughput than imaging-based methods as it sequences the entire transcriptome.  However, these technologies tend to have lower resolution (about 2-10 microns) compared to their optical counterparts, but can detect expression even on subcellular levels (in some cases).[^3][^4][^5] 

The following descriptions focus on NGS methods, specifically 10x Genomics Visium, and we want to show the power of NGS techniques as a research tool that provides novel context to single-cell and RNA expression data. We will explore the methods behind Visium technologies, the wide range of downstream analyses it enables, and ultimately their application within a larger study.

### Methods - Exploring the Technique Through to Visium

<p align="left">
  <em>Main Reference: (<a href="https://www.10xgenomics.com/support/spatial-gene-expression-fresh-frozen/user-guides?menu%5BproductNames%5D=Spatial%20Gene%20Expression%20for%20Fresh%20Frozen">Link</a>)</em>
</p>

NGS-based spatial transcriptomics techniques vary, but generally follow the same general principles of direct mRNA capture from the tissue, followed by sequencing. These approaches will be explored through observing a simplified and general protocol for 10x Genomics Visium. 

Sample Preparation: Fresh frozen samples at ≤ 6.5 x 6.5 mm are sectioned at 10 μm and directly mounted on the Visium slide as flat as possible. Though exact specifications vary between techniques, the Visium slides contain 2 by 2 μm pixels allowing us to distinguish each region through attached anchor sequences.8 These sequences each contain:
1. PCR adaptors for later pre-sequencing amplification[^2]
2. A barcode sequence: each pixel is distinguishable through a unique barcode of many bases.[^2]
3. UMI region: Unique Molecular Identifier, used to correct PCR bias.[^2]
4. Poly-T sequence: A sequence of many thymines, used in mRNA binding.[^2]

![Structure of Visium Slide](https://github.com/richardma2005/BENG-183-SPATIAL-TRANSCRIPTOMICS/blob/main/VisiumSlide.png)

<p align="center">
  <em>Above: Structure of Visium Slide (Fig (<a href="https://pubs.acs.org/doi/10.1021/jacsau.4c00118">Source</a>)</em>
</p>

#### Imaging

Tissues are then fixed and stained for microscopic imaging; the subsequent images will later be used to map our sequencing results directly to the slide array. There is flexibility in fixation, staining, and imaging methods, but overall it has to be done carefully to preserve the quality of the sample and the RNA within it.[^8]

#### Sequencing

After being placed on the Visium slide, the tissue is permeabilized, allowing RNA diffusion. At their poly-T sites, the anchor sequences can then bind mRNAs by their poly-A tails. Reverse transcription is then performed on the subsequent anchor-mRNA complex to generate cDNA with the barcode sequence.[^2][^4] Tissue is then digested to isolate the cDNA on the Visium Slides, and the cDNA is amplified and prepared into sequencing libraries.[^4][^8] Sequencing via NGS is then performed. Our final sequences contain both the spatial information of the barcode and the expression information of the captured mRNA.

![Workflow](https://github.com/richardma2005/BENG-183-SPATIAL-TRANSCRIPTOMICS/blob/main/VisiumWorkFlow.png)

<p align="center">
  <em>Above: Overview of Visium Workflow (Shi 2024, Figure 7A) (<a href="https://www.10xgenomics.com/blog/your-introduction-to-visium-hd-spatial-biology-in-high-definition">Source</a>)</em>
</p>

By the end of the process, we gain insights into the expression of each labeled/barcoded region of our sample, with each sequenced strand directly tied to a particular tissue area. In other terms, spatial transcriptomics, while harnessing the same principles of RNA capture and cDNA sequencing as other sequencing technologies, provides additional information with each strand. In turn, this enables a wide variety of data analysis applications, especially when consolidated with the higher resolution of single-cell sequencing. 

### Data Analysis

The central product of spatial transcriptomic analysis is a gene expression matrix, tied to each barcode sequence. In preprocessing, the image is aligned to a grid, with it being divided into spatial indices, tied to each barcode, developing a location matrix. Using the RNA strands, we create a gene expression matrix, in which the rows correspond to each mRNA. The columns correspond to each of the barcoded regions. Tools, such as Space Ranger, allow us to match these two matrices together. Downstream analysis in spatial transcriptomics can be split into a few categories: clustering, cell-type mapping, spatial gene detection, special gene inference, and modeling.[^1] The following examples illustrate tools and analytical approaches within these categories and are visualized in the figure below.

#### Clustering

Through methods such as K-means and hierarchical clustering, we can group gene-expression profiles of each region into clusters. Such clusters may subsequently indicate differences in cell type and function within the tissue or even allow us to distinguish and map cancerous and non-cancerous cell varieties.[^1][^3] Prior dimensionality reduction and batch-effect correction are recommended to reduce noise and improve accuracy. Example tools are tSNE and UMAP.[^1]

#### Integration of Single Cell Data

By tying our spatial transcriptomics to single-cell transcriptomic data, we develop a more comprehensive transcriptomic profile of our sample on the scale of individual cells. If our spatial transcriptomic results are on the scale of individual cells, we can directly use annotations within the single-cell data to map it to specific spatial regions. Otherwise, we employ machine learning to estimate the proportion of cell types and their relative amount of gene expression for each pixel. In the end, we gain a cell-type annotation of our spatial transcriptomics results, providing a spatial characterization of the cell-types within our, which is used in the proceeding analyses.[^1]

#### Spatially Variable Genes

We can note particular genes that significantly vary in expression throughout different areas of the tissue through deep learning and statistical modeling tools, including Giotto and SpatialIDE.[^1]

#### Spatial Regions

Cell gene expression depends on its location in the tissue, signifying the importance of exploring tissue regions. After the patterns of spatially variable genes are detected, the following can be used. MULTILAYER compares the spatial localization of gene expression to observe the extent of co-expression. RESEPT maps transcriptomic data to RGB images to infer and visualize their organizational structure.[^1]

#### Cell-Cell Interaction

Single-cell RNA sequencing can be used with spatial sequencing data to infer the degree of cell-to-cell interactions. Indeed tools such as via SpaOTsc enable us to develop a map between these two data sets. Paired with cell interaction data sets, this pairing allows us to infer interactions between coordinated cell environments, such as between tissue regions. This can be done using DIALOGUE, which treats coordinated cells with higher order structure as a singular functional unit to identify multicellular patterns.[^1]

#### Gene-Gene Interaction

With the integration of spatial and single-cell transcriptomic data allowing us to zoom into the molecular processes behind cell-cell interaction, gene-gene interactions (both intercellular and intracellular) can be inferred. Indeed, spatial transcriptomics enables the analysis of the role of extracellular interactions in gene expression. Tools such as GCN, ScHOT, MISTy, and MESSI help to analyze these interactions and help us infer how genes interact across cells and tissue contexts.[^1]

#### Spatial Trajectory

These analyses help us infer how cells evolve or differentiate by combining spatial coordinates with changes in gene expression. Using pseudo-time techniques to infer cell differentiation trajectories and gradients in space, tools such as StLearn and SPATA help to reconstruct dynamic biological processes within tissue architecture.[^1]

#### 3D Model Construction

Current spatial transcriptomics technologies only capture 2D spatial data. Therefore, 3D models are built computationally by aligning sequential tissue sections. The most common way is to use machine learning and statistical methods and frameworks. For example, STUtility aligns with the iterative nearest point algorithm, and PASTE applies Optimal Transport matching to construct 3D alignment.[^1]

<p align="center">
  <img src="https://github.com/richardma2005/BENG-183-SPATIAL-TRANSCRIPTOMICS/blob/main/DownstreamDataAnalyses.jpg" width="800">
</p>
<p align="center">
  <em>
    Above: Downstream analyses of spatial transcriptomics data (Figure 3, Liangchen Yue et al.) 
    (<a href="https://doi.org/10.1016/j.csbj.2023.01.016">Source</a>)
  </em>
</p>


As seen through this large list of applications, spatial transcriptomics enables a powerful collection of data analysis procedures, allowing us to reconstruct the active processes of tissue architecture; it not only allows us to view the spatial elements of a tissue, but also the dynamics among those elements. Single-cell data alone only provides a snapshot of the processes within a given cell type, but with spatial transcriptomics we see these cells in the context of other cell types, allowing us to elucidate the spatial processes which underlie their development and the intercellular influences on their gene expression. We analyze not just a tissue, but a tissue in action, building its overall cellular environment. 

### Applications

Having outlined how Visium preserves spatial information and captures transcriptomic information, we can now examine how these capabilities can be applied in practice. As seen above, spatial transcriptomics is a powerful tool to study tissue organization, development, and diseases with a level of detail that isn’t attainable through conventional sequencing alone. A clear example of this is the study by Qian et al. (2025), “Spatial transcriptomics reveals human cortical layer and area specification”, which integrates MERFISH, single-nucleus RNA sequencing (snRNA-seq), and Visium to create a spatially resolved single-cell atlas of the developing human cerebral cortex.

This study demonstrates how spatial transcriptomics is especially powerful in studying tissues where the physical location of cells is integral to their function, such as the developing brain. When studying the human brain and its cortical development, a major challenge is understanding how the fetal cortex establishes its distinct layers and functional areas over time. Although traditional single-cell RNA sequencing has greatly advanced our understanding of the molecular identities of developing cells, it still loses spatial context and cannot reveal where the cells are positioned physically. Spatial transcriptomics overcomes this challenge by preserving tissue structure and, therefore, capturing spatial information.

Qian addressed these developmental questions by combining spatial transcriptomics and single-cell approaches. MERFISH, an imaging-based spatial transcriptomics technique, was used to obtain a single-cell resolution spatial map of gene expression across the fetal cortex.[^5] snRNA-seq was used to obtain a complete and unbiased gene-expression profile for individual cell types.[^5] Visium was used to capture gene expression across the tissue in larger spots, which gives unbiased spatial context and enables authors to validate and extend patterns identified by MERFISH.[^5] Combining these three methods produced a comprehensive map of the developing cortex that shows both molecular identity and spatial position across developmental stages. 

In this context, Visium played an important supporting role in this study. It provided unbiased sequencing, which allowed researchers to confirm spatial patterns detected by MERFISH and to validate specific marker genes such as CDH13 and TRPC6, which differed between the primary visual cortex and secondary visual cortex.[^5] This demonstrates how Visium complements high-resolution imaging methods by providing spatial context. Overall, the Qian study shows why spatial transcriptomics is so powerful. By preserving the physical arrangement of the cells, these methods allow researchers to gain insight into organizational and developmental features that would otherwise remain hidden using conventional sequencing methods alone.

### Conclusion

From elucidating brain development to differentiating diseased cells, the veracity of spatial transcriptomics in inferring and validating gene expression pathways within a tissue sample proves evident. We see that the process of tying labeled/barcoded tissue regions to libraries of captured mRNAs permits us to analyze a tissue in action, in turn enabling a wide variety of applications in studying cell differentiation and communication, even within humans. As sequencing and imaging techniques continue to improve and databases expand, the increased power and resolution of spatial transcriptomics will continue to allow an even more comprehensive understanding of tissue and cell interactions that underlie biological systems.


### References

1. Yue, L., Liu, F., Hu, J., Yang, P., Wang, Y., Dong, J., Shu, W., Huang, X., & Wang, S. (2023). A guidebook of spatial transcriptomic technologies, data resources and analysis approaches. *Computational and Structural Biotechnology Journal*, 21, 940–955. https://doi.org/10.1016/j.csbj.2023.01.016

2. Shi, W., Zhang, J., Huang, S., Fan, Q., Cao, J., Zeng, J., Wu, L., & Yang, C. (2024). Next-Generation Sequencing-Based Spatial Transcriptomics: A Perspective from Barcoding Chemistry. *JACS Au*, 4(5), 1723–1743. https://doi.org/10.1021/jacsau.4c00118

3. Rao, A., Barkley, D., França, G. S., & Yanai, I. (2021). Exploring tissue architecture using spatial transcriptomics. *Nature*, 596(7871), 211–220. https://doi.org/10.1038/s41586-021-03634-9

4. Garam Kim. (2025, June 6). MPG Primer: Spatial Transcriptomics Technologies: A Primer. YouTube; Broad Institute. https://www.youtube.com/watch?v=JbKCH7omGV0

5. Qian, X., Coleman, K., Jiang, S., Kriz, A. J., Marciano, J. H., Luo, C., Cai, C., … Walsh, C. A. (2025). Spatial transcriptomics reveals human cortical layer and area specification. *Nature*, 644(8075), 153–163. https://doi.org/10.1038/s41586-025-09010-1

6. Chen, W. T., Lu, A., Craessaerts, K., Pavie, B., Sala Frigerio, C., … De Strooper, B. (2020). Spatial Transcriptomics and In Situ Sequencing to Study Alzheimer’s Disease. *Cell*, 182(4), 976–991.e19. https://doi.org/10.1016/j.cell.2020.06.038

7. Williams, C. G., Lee, H. J., Asatsuma, T., Vento-Tormo, R., & Haque, A. (2022). An introduction to spatial transcriptomics for biomedical research. *Genome Medicine*, 14(1). https://doi.org/10.1186/s13073-022-01075-1

8. 10x Genomics. (2025). User Guides | Official 10x Genomics Support. http://www.10xgenomics.com/support/spatial-gene-expression-fresh-frozen/user-guides?menu


[^1]: Yue, L., Liu, F., Hu, J., Yang, P., Wang, Y., Dong, J., Shu, W., Huang, X., & Wang, S. (2023). A guidebook of spatial transcriptomic technologies, data resources and analysis approaches. *Computational and Structural Biotechnology Journal*, 21, 940–955. https://doi.org/10.1016/j.csbj.2023.01.016

[^2]: Shi, W., Zhang, J., Huang, S., Fan, Q., Cao, J., Zeng, J., Wu, L., & Yang, C. (2024). Next-Generation Sequencing-Based Spatial Transcriptomics: A Perspective from Barcoding Chemistry. *JACS Au*, 4(5), 1723–1743. https://doi.org/10.1021/jacsau.4c00118

[^3]: Rao, A., Barkley, D., França, G. S., & Yanai, I. (2021). Exploring tissue architecture using spatial transcriptomics. *Nature*, 596(7871), 211–220. https://doi.org/10.1038/s41586-021-03634-9

[^4]: Garam Kim. (2025, June 6). MPG Primer: Spatial Transcriptomics Technologies: A Primer. YouTube; Broad Institute. https://www.youtube.com/watch?v=JbKCH7omGV0

[^5]: Qian, X., Coleman, K., Jiang, S., Kriz, A. J., Marciano, J. H., Luo, C., Cai, C., … Walsh, C. A. (2025). Spatial transcriptomics reveals human cortical layer and area specification. *Nature*, 644(8075), 153–163. https://doi.org/10.1038/s41586-025-09010-1

[^6]: Chen, W. T., Lu, A., Craessaerts, K., Pavie, B., Sala Frigerio, C., … De Strooper, B. (2020). Spatial Transcriptomics and In Situ Sequencing to Study Alzheimer's Disease. *Cell*, 182(4), 976–991.e19. https://doi.org/10.1016/j.cell.2020.06.038

[^7]: Williams, C. G., Lee, H. J., Asatsuma, T., Vento-Tormo, R., & Haque, A. (2022). An introduction to spatial transcriptomics for biomedical research. *Genome Medicine*, 14(1). https://doi.org/10.1186/s13073-022-01075-1

[^8]: 10x Genomics. (2025). User Guides | Official 10x Genomics Support. http://www.10xgenomics.com/support/spatial-gene-expression-fresh-frozen/user-guides?menu





