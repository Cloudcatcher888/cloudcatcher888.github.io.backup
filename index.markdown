---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---

<div align=center><img width = '150' height ='250' src ="images/portrait.jpeg"/></div>

# <center>Daniel Wong(王智恺)</center>



I am a PhD at SEIEE, Shanghai Jiao Tong University, where I was advised by Prof. Yanyan Shen. I will graduate in 2024. Before that, I obtained my bachelor degree at Shanghai Jiao Tong University in 2019.



# Research

I'm interested in devleoping efficient models for recommendation system (e.g. CTR prediction and sequential recommendation) and improve the generalization ability of models in incremental scenario.

## Conference Papers:

<div align=center><img width = '650' height ='250' src ="images/icde2.png"/></div>

1. **Incremental Learning for Multi-interest Sequential Recommendation**  [pdf]({{site.url}}/files/icde.pdf) [slides]({{site.url}}/files/icde_slides.pdf) 

   **Zhikai Wang**,Yanyan Shen

   *ICDE2023 (**best paper award**, accept rate: 19.3%)*

   **Abstract**: In recent years, sequential recommendation has been widely researched, which aims to predict the next item of interest based on user's previously interacted item sequence. Existing works utilize capsule network and self-attention method to explicitly capture multiple underlying interests from a user's interaction sequence, achieving the state-of-the-art sequential recommendation performance. In practice, the lengths of user interaction sequences are ever-increasing and users might develop new interests from new interactions, and a model should be updated or even expanded continuously to capture the new user interests. We refer to this problem as incremental multi-interest sequential recommendation, which has not yet been well investigated in existing literature. In this paper, we propose an effective incremental learning framework of multi-interest sequential recommendation called IMSR, which augments the traditional fine-tuning strategy with the existing-interests retainer (EIR), new-interests detector (NID), and projection-based interests trimmer (PIT) to adaptively expand the model to accommodate user's new interests and prevent it from forgetting user's existing interests. Extensive experiments on real-world datasets verify the effectiveness of the proposed IMSR on incremental multi-interest sequential recommendation, compared with various baseline approaches.

   

   <div align=center><img width = '300' height ='350' src ="images/ijcai.png"/></div>

2. **Feature Staleness Aware Incremental Learning for CTR prediction**  [pdf]({{site.url}}/files/ijcai23.pdf)

   **Zhikai Wang**,Yanyan Shen, Zibin Zhang, Kangyi Lin

   *IJCAI2023 (accept rate: 15%)*

   **Abstract**: Click-through Rate (CTR) prediction in real-world recommender systems often deals with billions of user interactions every day. To improve the training efficiency, it is common to update the CTR prediction model incrementally using the new incremental data and a subset of historical data. However, the feature embeddings of a CTR prediction model often get stale when the corresponding features do not appear in current incremental data. In the next period, the model would have a performance degradation on samples containing stale features, which we call the feature staleness problem. To mitigate this problem, we propose a Feature Staleness Aware Incremental Learning method for CTR prediction (FeSAIL) which adaptively replays samples containing stale features. We first introduce a staleness-aware sampling algorithm (SAS) to sample a fixed number of stale samples with high sampling efficiency. We then introduce a staleness-aware regularization mechanism (SAR) for a fine-grained control of the feature embedding updating. We instantiate FeSAIL with a general deep learning-based CTR prediction model and the experimental results demonstrate FeSAIL outperforms various state-of-the-art methods on four benchmark datasets.

   

   <div align=center><img width = '650' height ='250' src ="images/sdm.png"/></div>

3. [**Time-aware Multi-interest Capsule Network for Sequential Recommendation**](https://epubs.siam.org/doi/abs/10.1137/1.9781611977172.63)  [pdf]({{site.url}}/files/sdm.pdf)

   **Zhikai Wang**,Yanyan Shen

   *SDM2022 (accept rate: 28.7%)*
   
   **Abstract**: In recent years, sequential recommendation has been widely researched, which aims to predict the next item of interest based on user's previously interacted item sequence. Many works use RNN to model the user interest evolution over time. However, they typically compute a single vector as the user representation, which is insufficient to capture the variation of user diverse interests. Some non-RNN models employ the dynamic routing mechanism to automatically vote out multiple capsules that represent user's diverse interests, but they are ignorant of the temporal information of user's historical behaviors, thus yielding suboptimal performance. 
   In this paper, we aim to establish a time-aware dynamic routing algorithm to effectively extract temporal user multiple interests for sequential recommendation. We observe that the significance of an item to user interests may change monotonically over time, and user interests may fluctuate periodically. Following the intuitive temporal patterns of user interests, we propose a novel time-aware multi-interest capsule network named TAMIC that leverages two kinds of time-aware voting gates, i.e., monotonic gates and periodic gates, to control the influence of each interacted item on user's current interests during the routing procedure. We further employ an aggregation module to form a temporal multi-interest user representation which is used for next item prediction. Extensive experiments on real-world datasets verify the effectiveness of the time gates and the superior performance of our TAMIC approach on sequential recommendation, compared with the state-of-the-art methods.
   
3. **Contrastive learning for sequential recommendation: A survey**

   **Zhikai Wang**, Yanyan Shen

   (under review)
   
   
   
   <div align=center><img width = '650' height ='250' src ="images/ICIP.png"/></div>
   
4. [**Key joints selection and spatiotemporal mining for skeleton-based action recognition**](https://ieeexplore.ieee.org/abstract/document/8451483) [pdf]({{site.url}}/files/ICIP.pdf)
   
   **Zhikai Wang**, Chongyang Zhang, Wu Luo, Weiyao Lin
   
   *ICIP2018*
   
   **Abstract**: Trajectories and spatiotemporal attention model have been successfully used in skeleton-based action recognition. Most existing methods focus more attention on temporal structure mining. However, only a few local joints and their position features (e.g., critical position changes of hand, head, leg etc.) are responsible for the action label. In this work, we introduce a novel action recognition framework using Key Joints Selection and Spatiotemporal Mining, which can identify both key joints and their position & velocity histogram as well as trajectory features for action classification. First, histogram of human joints position and velocity are developed to enhance the spatiotemporal structure representation of existing trajectory-based methods. Second, the key joints are selected according to their information gains, and then their position & velocity histograms are weighted and composed with trajectory features to form one richer representation for final action classification. Experiments on two widely-tested benchmark datasets show that by combining the strength of both richer features and key joints selecting, our method can achieve state-of-the-art or competitive performance compared with existing results using sophisticated models such as deep learning, with advantages regarding the recognition accuracy and robustness.
