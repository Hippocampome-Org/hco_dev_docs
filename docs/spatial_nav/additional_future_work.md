Additional Possible Future Work
===============================

(Sutton et al., 2024) considered more complex interneuron connectivity methods to be out-of-scope of the work at its stage of research. This understanding is based on what animal recordings evidence was available at the time of publication. Possible future work exists in both animal recording and simulations to further explore what connectivity exists. This document describes some possible future work in addition to the future work described in the text of (Sutton et al., 2024).

Some relevant publications related to this connectivity are (Buetfering et al., 2014), (Dunn et al., 2017), and (Huang et al., 2023). (Dunn et al., 2017) references (Buetfering et al., 2014) and states that adding variability in grid cell firing can cause interneuron firing that adresses reported spatial selectivity but little to no spatial periodicity. (Buetfering et al., 2014)'s study reported a lack of spatial periodicity in interneurons. (Dunn et al., 2017)'s method of addressing interneuron firing involved "...we consider a two population network...with appropriate connectivity, field-to-field variability can lead to irregular firing, and that this comes at the cost of the ability to integrate velocity properly." (pg. 5). (Dunn et al., 2017)'s method was not used in (Sutton et al., 2024) due to the interest in incorperating velocity input into the simulation to help control grid cell firing. 

(Huang et al., 2023)'s study reports "we suggest that dense, but specific, direct excitatory-inhibitory synaptic interactions may operate at the scale of grid cell clusters, while indirect interactions may coordinate activity at the scale of grid cell modules." (pg. 1). This clustering of grid cells and interneuron connections that may occur indicates that interneurons may have specific connectivity with grid cells that possibly supports attractor dynamics as simulated in (Sutton et al., 2024). Methods used for animal recordings and experimental designs in (Buetfering et al., 2014) and (Huang et al., 2023) had some differences. Further work with animal recordings can help identify what specific connectivity exists and that can be used as a constraint in modeling.

The connectivity used in (Sutton et al., 2024)'s simulation could be considered an abstraction of more complicated connectivity if such complex connectivity is found to exist. Possibly, once more animal recording is done, it could be found that there are discrepancies in between the simulation's connectivity and recorded connectivity. In such a case additional elements could be added to the model could help resolve that. Additional elements could include recursive interneuron connections, additional interneuron groups, or additional forms of input being received by interneurons such as by other neuron groups. Additional animal connectivity evidence would be valuable to help narrow down the potential canidates for connectivity within the simulation if such findings are discovered.

### References

Christina Buetfering, Kevin Allen, and Hannah Monyer. Parvalbumin interneurons provide grid
cell-driven recurrent inhibition in the medial entorhinal cortex. Nature neuroscience, 17(5):710–718,
2014. https://www.nature.com/articles/nn.3696

Dunn, B., Wennberg, D., Huang, Z., & Roudi, Y. (2017). Grid cells show field-to-field variability and this explains the aperiodic response of inhibitory interneurons. arXiv preprint arXiv:1701.04893. https://doi.org/10.48550/arXiv.1701.04893

Huang, L. W., Garden, D. L., McClure, C., & Nolan, M. (2023). Synaptic interactions between stellate cells and parvalbumin interneurons in layer 2 of the medial entorhinal cortex are organized at the scale of grid cell clusters. bioRxiv, 2023-09. https://doi.org/10.1101/2023.09.18.558206

Sutton, N. M., Gutiérrez-Guzmán, B. E., Dannenberg, H., & Ascoli, G. A. (2024). A Continuous Attractor Model with Realistic Neural and Synaptic Properties Quantitatively Reproduces Grid Cell Physiology. International Journal of Molecular Sciences, 25(11), 6059. https://doi.org/10.3390/ijms25116059