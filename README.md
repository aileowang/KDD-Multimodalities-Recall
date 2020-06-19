## KDD-Multimodalities-Recall

This is our solution for KDD Cup 2020. We implemented a very neat and simple neural ranking model based on siamese BERT[1] which **ranked FIRST among the solo teams** and ranked 12th among all teams on the final leaderboard. ![visitors](https://visitor-badge.glitch.me/badge?page_id=chengsyuan.KDD-Multimodalities-Recall)

##### Related Project: [WSDM-Adhoc-Document-Retrieval](https://github.com/chengsyuan/WSDM-Adhoc-Document-Retrieval)

### Features	

* An end-to-end system with **zero feature engineering**.
* Implemented the model using **only 36 lines of code**.
* Performed data cleaning on the dataset according to self-designed rules, and removed the abnormal data with an significant negative impact on model performance. 
* Designed a siamese light-BERT with 4-layers to **avoid up to 83% unnecessary computation cost for both training and inference** comparing to the full-percision BERT model.
* **Training on fly**: our model can achieve SOTA performance in **less than 5 hours** using a single V100 GPU card.
* Using a **Local-Linear-Embedding-like method** to increase the NDCG@5 by ~1%.
* Scores are stable (nearly the same) on the validation, testA and testB.

### Our Pipeline

1. Open the jupyter notebook ```jupyter lab or jupyter notebook```
2. Clean the dataset: ```Open 01PRE.ipynb, change the variable 'filename' and run all cells```. In this notebook, we perform base64 decoding process and using BERT tokenizer to covert the queryies into token_id lists. We also convert the data type into float16 to further reduce both disk and memory usage.
3. Model Training: ```Open one of 02MODEL.ipynb and run all cells```. In this notebook, we remove the sample if its bounding box number is higher than 40, which is the also the maximum bounding box number of the test set. Then we use a siamese light-BERT with 4-layers to learning every (query, image) pairs sampled uniformly from the training set. A off-the-shelf framework - pytorch-lightning is used.
4. Inference: ```Open 03INFER.ipynb```. In this notebook, we rewrite the InferSet class to implement our local-linear-embedding-like method. By the way, the inference code is almost the same as validation code so we think it is unnecessay to provide our messy code :).

**Local-Linear-Embedding-like Method**

As mentioned in the step4, we adopted the local-linear-embedding-like method to further enhance the feature.

<img src="images/lle.png" alt="image-20200613151309784" style="zoom:33%;" width="280" height="300"/>

Given a ROI, we find the top3 most similar ROIs using KNN (K-nearest neighbour) method, then we summed them by weight 0.7, 0.2, 0.1 for keeping the same input numerical scale.

### Members

This is a solo team which consists of:

1. **Chengxuan Ying**, Dalian University of Technology (应承轩 大连理工大学)

### Acknowledgment

Thanks for [Weiwei Xu](http://www.cad.zju.edu.cn/home/weiweixu/weiweixu_en.htm), who provided a 4-GPU server.

### Links to Other Solutions

* [KDD_WinnieTheBest](https://github.com/steven95421/KDD_WinnieTheBest) (Ranked 1st on the LB)
* [Ai-Light](https://github.com/Ai-Light/KDD2020Multimodalities)

### Future Work

It is worthy to try the SOTA of image-text representation models, like UNITER[2] or OSCAR[3].

### Reference

1. Devlin J, Chang M W, Lee K, et al. Bert: Pre-training of deep bidirectional transformers for language understanding[J]. arXiv preprint arXiv:1810.04805, 2018.
2. Chen Y C, Li L, Yu L, et al. Uniter: Learning universal image-text representations[J]. arXiv preprint arXiv:1909.11740, 2019.
3. Li X, Yin X, Li C, et al. Oscar: Object-Semantics Aligned Pre-training for Vision-Language Tasks[J]. arXiv preprint arXiv:2004.06165, 2020.

### Seeking Opportunities

I will be graduated in the summer of 2021 from Dalian University of Technology. If you can refer me to any company, please contact me [yingchengsyuan@gmail.com](mailto:yingchengsyuan@gmail.com).

