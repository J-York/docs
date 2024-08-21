Dear Reviewers：

Thank you for the valuable feedback and constructive comments on our manuscript titled "End-to-End AutoML for Unsupervised Log Anomaly Detection". Your insights have played a crucial role in helping us improve the clarity and quality of our research. In response to your suggestions, we have made several revisions to the manuscript, which we believe have strengthened our arguments and enhanced the overall presentation. For your convenience, the revised text has been highlighted in blue.

In the following responses, we will use italicized text to indicate direct quotes from the reviewers' comments, while our replies will be presented in regular text. The quoted portions refer to the text in the revised manuscript.

------

**Response to Meta Reviewer #1664D**

**Comment1** : *The first concern is about the search space, which is very small in this paper. Therefore, we don't even need a search algorithm for it. We ask the authors to expand their search space to around 1 million combinations, which is common in search-based SE, and compare with some common search algorithms like genetic algorithms or particle swarm optimization to evaluate and prove the necessity and effectiveness of the proposed approach.*

**Response** : We have expanded the hyperparameter search space to the tens of millions level and applied the Particle Swarm Optimization(PSO) algorithm as a coarse screening process for the recommendation system before meta-learning.

In **Section 4.2**, we provide a detailed description of the framework's structure after incorporating the hyperparameter search algorithm, and in **Table 4**, we present the search ranges of our hyperparameters, their meanings, and the corresponding models.

> To ensure that our framework performs well on different datasets, we initialized the hyperparameter search space across a wide range. The range of each hyperparameter and the corresponding models are shown in Table 4, resulting in a total of 11,136,000 algorithm and hyperparameter combinations. To select models that are likely to perform well on the target dataset under the huge searching space and reduce the cost of training the meta-learner, we first run the Particle Swarm Optimization algorithm (PSO) [24] on the labeled support set. 

> For each algorithm, we select the top 1000 hyperparameter combinations based on their performance on each support set and use these models as the candidate set for the meta-learner. After removing those identical models, we obtained a candidate set of models with a size of 7840. The subsequent task is to recommend the most suitable model on a new unlabeled dataset.

In **Section 5.3**, we compared the time required to obtain candidate sets of the same size using grid search, genetic algorithms, and PSO, along with the performance of the resulting candidate sets. This comparison confirms that the PSO algorithm can achieve satisfactory hyperparameter search outcomes within an acceptable runtime. The final experimental results are presented in **Table 7**.

> To study the impact of different hyperparameter algorithms on the quality of the candidate set for meta-learning models, we com- pared three hyperparameter selection algorithms: Random Search [4], Genetic Algorithm [22], and Particle Swarm Optimization [24]. We recorded the time required by these algorithms from the start of hyperparameter selection to obtaining a candidate set of a specified size, and we generated candidate sets using each of the three algorithms. 



**Comment2** : *The second concern is about the generality of the approach. For now, it is only evaluated with LogBERT, which is based on the transformer architecture. We ask the authors to evaluate their method with more model architectures, including LSTM and CNN.*

**Response :** We have reproduced a CNN-based anomaly detection method and selected DeepLog, LogAnomaly, LogBERT, and CNN as candidate algorithms for LogCraft.

In **Section 4.2**, we introduce the model search space of LogCraft, which includes two LSTM-based algorithms (DeepLog and LogAnomaly), one CNN-based algorithm, and one Transformer-based algorithm (LogBERT).

> During this phase, we define the search space consisting of various models. Each model is a combination of features, al- gorithms, and hyperparameters. Specifically, the features include event vectors, count vectors, and semantic vectors, as well as their combinations. The algorithms encompass four unsupervised log anomaly detection algorithms: DeepLog [12], LogAnomaly [35], CNN [7],and LogBERT [15].

In **Section 5.2**, we present the final experimental results, which indicate that LogCraft recommends different base algorithms for different datasets.

> The experimental results, as shown in Table 6, demonstrate the effectiveness of LogCraft in achieving the highest F1 scores on the BGL, Spirit, HDFS, and ThunderBird datasets. Specifically, for the detection tasks of BGL and Liberty, LogCraft recommended a model based on LogBERT; for the detection tasks of HDFS and Spirit, LogCraft selected a model based on DeepLog; and for the Thun- derBird dataset, LogBERT opted for a CNN model with adjusted parameters for detection.

**Comment3 :** *The third concern is about the training set. The authors need to make it smaller (less than 10%), which is more practical. As this setting can bring bias to experiments, the authors need to repeat the experiments several times and report the average results.*

**Response** :  In the latest experiment, we standardized LogCraft with other baseline experiments, sampling only 10% of the log dataset for anomaly detection. For each task, we conducted three experiments and used the average value as the final detection result.

For LogCraft and other baseline algorithms in Section 5, we used the same data settings, specifically selecting 10% of the normal logs through random sampling for training, while the remaining logs were used for detection. Accordingly, we updated **Tables 6 to 8** and **Figures 3 to 5**.

> For the target set, we randomly selected 10% of the normal logs for model training, with the remainder normal logs and all abnormal logs used for anomaly detection.

> We conducted three experiments starting from the construction of the meta-learner and took the average as the final result.

------

We hope that the revisions made address your concerns and improve the manuscript significantly. Thank you once again for your thoughtful critique and support. 

Sincerely,

Authors of ASE2024 Paper#1664 "End-to-End AutoML for Unsupervised Log Anomaly Detection"
