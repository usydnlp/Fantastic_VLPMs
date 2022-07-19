# Fantastic VLPMs
This repository builds upon our recent [survey paper for **Vision-Language Pretraining Models (VLPMs)**](https://arxiv.org/pdf/2204.07356.pdf) pusblished in IJCAI 2022. It contains a list of papers (codes if available) and pretraining/downstream task datasets for the existing VLPMs, currently with main focus on *Text* and *Image*. We will keep maintaining and adding on new VLPM research and works. 

<h4 align="center">
  <b>Long, S., Cao, F., Han, C., & Yang, H. (2022). <br/><a href="https://arxiv.org/pdf/2204.07356.pdf">Vision-and-Language Pretrained Models: A Survey</a><br/> (IJCAI 2022), pp. XXX</b></span>
</h4>

If you find any error or have any suggestion, please don't hesitate to open an issue and contact us.
If you find this repository helpful for your research or work, please kindly cite our VLPM survey paper pusblished in IJCAI 2022 using the Bibtex provided below:

```
@article{long2022vision,
  title={Vision-and-Language Pretrained Models: A Survey},
  author={Long, Siqu and Cao, Feiqi and Han, Soyeon Caren and Yang, Haiqing},
  journal={arXiv preprint arXiv:2204.07356},
  year={2022}
}
```

# Introduction
So far, pretraining models have produced great success in both Computer Vision and Natural Language Processing. While CV researchers use VGG and ResNet predict the categorical label of a given image, BERT has been used and revolutionised many NLP tasks, such as natural language inference, and reading comprehension. Motivated by this, many cross-modal Vision-Language pretraining models, i.e., VLPMs, have been designed and trained by feeding visual and linguistic contents into the multi-layer transformer.

This repo builds on our survey and systematically organizes the existing VLPMs in terms of related papers (with their codes if available), and the commonly used pretraining and downstream task datasets, aiming to provide a general guideline and reference for future research development in VLPMs. 

# Structure of this repo
- [Vision-and-Language Pretrained Models: A Survey](#VLSurvey)
  - [Generalized VLPM architecture](#VLSStruc)
  - [Input Encoding - Raw V/L Input and V/L Representation](#VLSIE)
  - [V and L Interaction Model (V-LIM)](#VLSVLIM)
  - [Pretraining tasks and dataset](#VLSPTD)
  - [Downstream tasks and dataset](#VLSDTD)
  - [Future Research Direction](#VLSFRD)
- [Comprehensive Overview](#Comprehensive)
  - Paper with code (only listed if available) ordered by year
  - Paper with methodlogies - Table 1 in the journal version
  - Paper with tasks and datasets - Table 2 in the journal version (add downstream tasks?)

<h1 id="VLSurvey">
Vision-and-Language Pretrained Models: A Survey
</h1>

<h2 id="VLSStruc">
Generalized VLPM architecture
</h2>

To present the review in a structured way, we generalized the VLPM into four major components, as is illustrated in the figure below (Figure 1 in our survey paper): 

<p align="center"><img src="https://github.com/usydnlp/Fantastic_VLPMs/blob/main/img/general_arc.png" alt="Generalized Architecture" width="600"/></p>

 __*1) V or L Raw Input Data*__: defines the representative raw data streams from the single modality contents, being either vision or language.
 
 __*2) V or L  Representation*__: processes the raw data input into the desired format of modality representations.
 
 __*3) V and L Interaction Model*__: enforces the cross-modal modelling between the two modality representations.
 
 __*4) V and L Representation*__: defines the possible cross-modal representations derived from the cross-modal modelling. 
 

We systematically review the VLPM architectures in terms of those four components, with main focus on multi-modal handling perspective. Based on this, we then summarise the pretraining strategies and downstream evaluation tasks accordingly.

<h2 id="VLSIE">
Input Encoding - Raw V/L Input 
</h2>





<h2> Input Encoding - V/L Representation </h2>

In the following table, we summaries the granularities of **V representation (visual representation)** applied by existing VLPMs, i.e. how image pixels are split into groups as visual tokens. It is an important decision of design since it decides the alignment level of cross-modal modeling in the image content, i.e. the source of cross-modal interaction 



<table>
    <thead>
        <tr>
            <th style="width: 10%">Granularity</th>
            <th style="width: 10%">Definition</th>
            <th style="width: 25%">Intution</th>
            <th style="width: 20%">Visual Embedding</th>
            <th style="width: 20%">Spatial Position Embedding</th>
            <th>Example VLPMs</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ROI-based</td>
            <td>Object regions as visual tokens</td>
            <td>Assume most image-text pairwise data is supposed to have its text describe the salient object regions in the corresponding image </td>
            <td>Visual feature extracted by pre-trained Faster R-CNN object detector</td>
            <td>Coordinates of object regions, e.g. 5d vector (normalized ROI bbox and the fraction of image area)
</td>
            <td>Vilbert (Lu et al., 2019) </td>
        </tr>
        <tr>
            <td>Patch-based</td>
            <td>Grid-patches as visual tokens</td>
            <td rowspan=2><li>Seek for more fine-grained cross-modal alignment</li><li>No dependency on object detector (as in ROI-based VLPMs)</li><li>Efficient for end-to-end visual representation learning</li></td>
            <td rowspan=2><li>Mostly, visual feature extracted by CNN-based extractor</li><li>More recently, shallow and convolution-free embedding that utilizes simple linear projection for encoding</li></td>
            <td rowspan=2>Pixel location, e.g. 2d-aware vector for row/column number</td>
            <td>Fashionbert (Gao et al., 2020)</td>
        </tr>
        <tr>
            <td>Pixel-based</td>
            <td>Rows of pixels as visual tokens</td>
            <td>Pixel-BERT (Huang et al., 2020) </td>
        </tr>
        <tr>
            <td>Image-based</td>
            <td>Complete images as visual tokens</td>
            <td><li>Driven by multi-image based tasks such as Vision-Language Navigation (VLN)</li><li>Focus more on inter-image correlation, angle change in the images of the same scene</li></td>
            <td>Pooled visual feature from CNN model, e.g. EfficientNet</td>
            <td>Coordinates of object regions, e.g. 5d vector (normalized ROI bbox and the fraction of image area)
</td>
            <td>PREVALENT (Hao et al., 2020) </td>
        </tr>
    </tbody>
</table>


<h3>Intra-modality Processing</h3>

Before feeding the Bert-formatted language/visual representation into the cross-modal interaction model, some VLPMs further conduct additional **intra-modality processing** to the language or visual representation (or both). Most VLPMs adopt additional self-attention based transformer blocks as intra-modality processing. Several VLPMs also apply non-transformer processing such as using AoANet and the Visual Dictionary-based mapping. Transformer blocks provides flexibility of selecting a different number of blocks for the two modalities. In practice, the language modality normally applies more blocks than the vision modality for a better balance. 

<p align="center"><img src="https://github.com/usydnlp/Fantastic_VLPMs/blob/main/img/intra-modealprocessing.png" alt="Intra-modal Porcessing" width="900"/></p>



<h2 id="VLSVLIM">
V and L Interaction Model (V-LIM)
</h2>

So far, there are two types of mainstream VLPM model structures in the existing literature:

<p align="center"><img src="https://github.com/usydnlp/Fantastic_VLPMs/blob/main/img/ExistingVLPMmainstreams.png" alt="Intra-modal Porcessing" width="900"/></p>

- *Single-stream (1-stream)*: directly fuse the Bert-formatted language/visual representation by using the joint cross-modal encoder at the initial stage
- *Double-stream (2-stream)*: separately apply the intra-modality processing to two modalities along with a shared cross-modal encoder

However, this way of classification is mainly based on the perspective of data stream and intra-modality data handling before the cross-modal modelling. 

**In this survey, we propose three main categories with focus on the Vision-Language Interaction Modelling (V-LIM), hoping to enlighten a new perspective on cross-modal alignment learning strategy.**

<p align="center"><img src="https://github.com/usydnlp/Fantastic_VLPMs/blob/main/img/VLIMs.png" alt="3 types of V-LIMs" width="900"/></p>

**1) Self-attention-based V-LIM**: directly apply single-stream self-attention module to the modality representations for cross-modal modeling, e.g. most single-stream VLPMs and some of the double-stream VLPMs 

**2) Co-attention-based V-LIM**: decouple the intra- and cross- modal modeling processes, which keep two separate streams of transformer blocks for each modality that are entangled only at the specific cross-attention sub-layer, e.g. double-stream VLPMs

**3) VSE-based V-LIM**: simply utilize the dual-encoder structured model with Visual-Semantic-Embedding(VSE)-based cross-modal contrastive learning. It is mostly applied for retrieval task-focused VLPMs

We categorize those encoder-decoder VLPMs as either 1) or 2) or combination of both depending on the type of attention mechanism is applied between the two modalities.

The table below provides the brief summarization of the three types of V-LIMs.

<table>
    <thead>
        <tr>
            <th style="width: 10%">V-LIM</th>
            <th style="width: 30%">Cross-modal Modeling Strategy</th>
            <th style="width: 25%">Example VLPMs</th>     
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Self-attention-based</td>
            <td>Enforce simultaneous intra- and cross-modal modeling via self-attention</td>
            <td>Visualbert (Li et al., 2019) [single-stream] <br> Pixel-bert (Huang et al., 2021) [double-stream]</td>
        </tr>
        <tr>
            <td>Co-attention-based</td>
            <td><li>Decouple intra- and cross-modal interaction while having focus on both</li><li>Allow flexible design of modeling depth on separate intra- and cross-modal modeling</li></td>
            <td>Ernie-vil (Yu et al., 2021) [double-stream] <br> Lxmert (Tan and Bansal, 2019) [double-stream]</td>
        </tr>
        <tr>
            <td>VSE-based</td>
            <td><li>Eradicate the fusion-style attention-based V-LIM, which provides efficiency</li><li>Enable independent V/L representation encoding of both modalities, making pre-computation possible for more efficient retrieval</li></td>
            <td>ALIGN (Jia et al., 2021) <br> LightningDOT (Sun et al., 2021)</td>
        </tr>
    </tbody> 
</table>


<h2 id="VLSVLIM">
V and L Representation
</h2>

The design of the Vision or Language and Vision-language representation depend on the model structure whereas their usage depends on the goal of pretraining and downstream tasks.

<table>
    <thead>
        <tr>
            <th style="width: 10%">V-LIM</th>
            <th style="width: 30%">V/L Representation</th>
            <th style="width: 25%">V-L Representation</th>   
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Self-attention-based</td>
            <td>Output representation of the language or vision sequence from the joint cross-modal encoder</td>
            <td>Output representation of the global-level special token, such as [CLS]</td>
        </tr>
        <tr>
            <td>Co-attention-based</td>
            <td>Output representation of the two separate modality streams</td>
            <td>Output representation of the global-level special token of either modality stream or their aggregation</td>
        </tr>
        <tr>
            <td>VSE-based</td>
            <td>Independent V/L representation from the separate language and visual encoder</td>
            <td>Fused representation of V/L representation</td>
        </tr>
    </tbody>
</table>

<h2 id="VLSPTD">
Pretraining tasks and dataset
</h2>

<h2 id="VLSDTD">
Downstream tasks and dataset
</h2>

<h2 id="VLSFRD">
Future Research Direction
</h2>

<h1 id="Comprehensive">
Comprehensive Overview
</h1>

<h2 id="CompCode">
Code availability overview
</h2>

<h2 id="CompMethod">
Methodology summary overview
</h2>

<h2 id="CompTaskData">
Tasks and dataset summary overview
</h2>


![image](https://github.com/usydnlp/Fantastic_VLPMs/blob/main/img/vlpm_ontology.png)
