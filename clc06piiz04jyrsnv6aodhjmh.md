# How I passed the AWS Certified Machine Learning — Specialty Exam (MLS-C01)

## Introduction

- I recently passed the AWS Certified Machine Learning — Specialty Exam. I'd like to share my thoughts on what I did to pass and some notes I kept along the way. Before beginning, it's worth considering what this exam is about.

- AWS Certified Machine Learning - Specialty is intended for individuals who perform a development or data science role and have more than one year of experience developing, architecting, or running machine learning/deep learning workloads in the AWS Cloud.

- Earning AWS Certified Machine Learning - Specialty validates expertise in building, training, tuning, and deploying machine learning (ML) models on AWS. This exam helps organizations identify and develop talent with critical skills for implementing cloud initiatives. 

- This article gives you an overview which content and training material I used to prepare myself for the AWS MLS-C01 exam.

## Exam Prerequisites

Before you take this exam, AWS recommends you have:

- At least two years of hands-on experience developing, architecting, and running ML or deep learning workloads in the AWS Cloud
- Ability to express the intuition behind basic ML algorithms
- Experience performing basic hyperparameter optimization
- Experience with ML and deep learning frameworks
- Ability to follow model-training, deployment, and operational best practices

## Exam overview

**Level:** Specialty

**Length:** 180 minutes to complete the exam

**Cost:** 300 USD

Visit[Exam pricing](https://aws.amazon.com/certification/policies/before-testing/#Exam_pricing) for additional cost information.

**Format:** 65 questions, either multiple choice or multiple response

**Delivery method:** Pearson VUE and PSI; testing center or online proctored exam

## Exam Outline

- This exam guide includes weightings, test domains, and objectives for the exam. It is not a comprehensive listing of the content on the exam. However, additional context for each of the objectives is available to help guide your preparation for the exam. 

- The following table lists the main content domains and their
weightings. The table precedes the complete exam content outline, which includes the additional context. The percentage in each domain represents only scored content.


| Domain  | % of Exam |
| ------- | --------- |
|Domain 1: Data Engineering |20%|
|Domain 2: Exploratory Data Analysis |24%|
|Domain 3: Modeling |36%|
|Domain 4: Machine Learning Implementation and Operations |20%|
|TOTAL |100%|

**Domain 1: Data Engineering**
1.1 Create data repositories for machine learning.
 Identify data sources (e.g., content and location, primary sources such as user data)
 Determine storage mediums (e.g., DB, Data Lake, S3, EFS, EBS)
1.2 Identify and implement a data ingestion solution.
 Data job styles/types (batch load, streaming)
 Data ingestion pipelines (Batch-based ML workloads and streaming-based ML workloads)
o Kinesis
o Kinesis Analytics
o Kinesis Firehose
o EMR
o Glue
 Job scheduling
1.3 Identify and implement a data transformation solution.
 Transforming data transit (ETL: Glue, EMR, AWS Batch)
 Handle ML-specific data using map reduce (Hadoop, Spark, Hive)

**Domain 2: Exploratory Data Analysis**
2.1 Sanitize and prepare data for modeling.
 Identify and handle missing data, corrupt data, stop words, etc.
 Formatting, normalizing, augmenting, and scaling data
 Labeled data (recognizing when you have enough labeled data and identifying mitigation strategies [Data labeling tools (Mechanical Turk, manual labor)])
2.2 Perform feature engineering.
 Identify and extract features from data sets, including from data sources such as text, speech, image, public datasets, etc.
 Analyze/evaluate feature engineering concepts (binning, tokenization, outliers, synthetic features, 1 hot encoding, reducing dimensionality of data)
2.3 Analyze and visualize data for machine learning.
 Graphing (scatter plot, time series, histogram, box plot)
 Interpreting descriptive statistics (correlation, summary statistics, p value)
 Clustering (hierarchical, diagnosing, elbow plot, cluster size)

**Domain 3: Modeling**
3.1 Frame business problems as machine learning problems.
 Determine when to use/when not to use ML
 Know the difference between supervised and unsupervised learning
 Selecting from among classification, regression, forecasting, clustering, recommendation, etc.
3.2 Select the appropriate model(s) for a given machine learning problem.
 Xgboost, logistic regression, K-means, linear regression, decision trees, random forests, RNN, CNN, Ensemble, Transfer learning
 Express intuition behind models
3.3 Train machine learning models.
 Train validation test split, cross-validation
 Optimizer, gradient descent, loss functions, local minima, convergence, batches, probability, etc.
 Compute choice (GPU vs. CPU, distributed vs. non-distributed, platform [Spark vs. non-Spark])
 Model updates and retraining
o Batch vs. real-time/online
3.4 Perform hyperparameter optimization.
 Regularization
o Drop out
o L1/L2
 Cross validation
 Model initialization
 Neural network architecture (layers/nodes), learning rate, activation functions
 Tree-based models (# of trees, # of levels)
 Linear models (learning rate)
3.5 Evaluate machine learning models.
 Avoid overfitting/underfitting (detect and handle bias and variance)
 Metrics (AUC-ROC, accuracy, precision, recall, RMSE, F1 score)
 Confusion matrix
 Offline and online model evaluation, A/B testing
 Compare models using metrics (time to train a model, quality of model, engineering costs)
 Cross validation

**Domain 4: Machine Learning Implementation and Operations**
4.1 Build machine learning solutions for performance, availability, scalability, resiliency, and fault tolerance.
 AWS environment logging and monitoring
o CloudTrail and CloudWatch
o Build error monitoring
 Multiple regions, Multiple AZs
 AMI/golden image
 Docker containers
 Auto Scaling groups
 Rightsizing
o Instances
o Provisioned IOPS
o Volumes
 Load balancing
 AWS best practices
4.2 Recommend and implement the appropriate machine learning services and features for a given problem.
 ML on AWS (application services)
o Poly
o Lex
o Transcribe
 AWS service limits
 Build your own model vs. SageMaker built-in algorithms
 Infrastructure: (spot, instance types), cost considerations
o Using spot instances to train deep learning models using AWS Batch
4.3 Apply basic AWS security practices to machine learning solutions.
 IAM
 S3 bucket policies
 Security groups
 VPC
 Encryption/anonymization
4.4 Deploy and operationalize machine learning solutions.
 Exposing endpoints and interacting with them
 ML model versioning
 A/B testing
 Retrain pipelines
 ML debugging/troubleshooting
o Detect and mitigate drop in performance
o Monitor performance of the model

👉 More Information on the exam guide can be found [here](https://d1.awsstatic.com/training-and-certification/docs-ml/AWS-Certified-Machine-Learning-Specialty_Exam-Guide.pdf).

## How did I prepare ?

I spend a considerable amount of time learning AWS before attempting to take the Certification Exam. It is important that you spend time with AWS for you to be good at it. The Resources I used while preparing I am linking them below one by one.

1) **📚 Courses I took:** Initially, I enrolled in a course on Udemy called “AWS Certified Machine Learning Specialty” by Stephane Maarek and Frank Kane which is a very good course and covers all the most important aspects of the AWS and helps you learn SageMaker, feature engineering, data engineering, modeling & more. 

👉 More Information on the udemy course can be found [here](https://www.udemy.com/course/aws-machine-learning/)

2)**🛠️ hands-on projects:** Learning only theory won't help , you must work on some hands-on AWS projects. I would recommend you to practice some of the AWS projects from [here](https://awsworkshop.io/) or you can practice them from [skills builder learning Center](https://skillbuilder.aws/).

👉some of the projects I practiced are mentioned in my [github repository](https://github.com/AditModi). here is the [list of projects](https://github.com/My-Machine-Learning-Projects-CT) I worked on for my AWS Machine learning Exam.

3)**📋 AWS Ramp-Up Guides:** Your guides to learning the AWS Cloud.

- AWS Ramp-Up Guides offer a variety of resources to help you build your skills and knowledge of the AWS Cloud. Each guide features carefully selected digital training, classroom courses, videos, whitepapers, certifications, and more. Explore the guides below by role, solution, or industry area.

👉 more details can be found [here](https://aws.amazon.com/training/ramp-up-guides/)

👉 Focus on machine learning based [solution guide](https://d1.awsstatic.com/training-and-certification/ramp-up_guides/Ramp-Up_Guide_Machine_Learning.pdf).

4) **🤝 being part of a Study Groups:** I also recommend you to be part of a study groups. it helps you stay focused, probably having study groups with people studying for the same exam is an added benefit.

Study Groups I was part of:

- Cloud and DevOps Babies:

Cloud and DevOps Babies are a global group of babies with curious minds to learn/decode Cloud, DevOps and Microservices tech stacks. 

👉 more details can be found [here](https://www.linkedin.com/company/devopsandcloudbabies/)

- Tech Study Slack: 
TechStudySlack is a Slack for people studying Tech

👉 more details can be found [here](https://techstudyslack.com/)

5)**✍️ practice tests:** Lastly I recommend all of you to pass these practice exams before attending the real exam. It provides simulated questions that are very similar to the actual exam.

One of the selling points of this practice exam is that each question contains detailed explanations that will help you gain a deeper understanding of AWS services. It not just explains what the correct answer is, but also explains why other answers are wrong. It is extremely helpful to make you recognize the difference between similar services.

- Tutorials Dojo Practice Exams:

👉 more details can be found [here](https://tutorialsdojo.com/courses/aws-certified-machine-learning-specialty-practice-exams/)

6)**📝 Notes:** I outlined the resources I would use and a rough guideline of how I would approach studying. You should find something that works for you but have structure and commit to it. 

## Useful Study tips and tricks

As usual, more study tips and tricks to help you with the exam:

- This exam is available through online proctoring so that you don’t have to travel to the nearby testing center to take this exam.
- Additional handicap of 30 minutes for non-native English speakers is still available, so make sure that you have requested that.
- Make sure you will use the flagging mechanism and reiterate the questions if you have time.
- There is no penalty for guessing.
- You can and should apply the same rules as in the other exams look [here]() for more details regarding how to read a question and answers.

## Additional Resources

- [Andrew Brown’s ExamPro AWS Certified Solutions Architect Course](https://www.freecodecamp.org/news/pass-the-aws-certified-solutions-architect-exam-with-this-free-10-hour-course/)

- [Stephane Mareek and Frank Kane’s AWS Certified Machine Learning Specialty Course](https://www.udemy.com/course/aws-machine-learning/)

- [Mike Chamber's AWS Certified Machine Learning — Specialty Course](https://mikegchambers.teachable.com/)

- [Noah Gift’s AWS-MLS-C01 Study Guide Github Repo](https://github.com/noahgift/aws-ml-guide)

I hope this will help you to prepare and evaluate your knowledge. 
Let me know your thoughts in the comment section 👇
And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on [LinkedIn](https://www.linkedin.com/in/adit-modi-2a4362191/)**
🤓 **connect with me on [Twitter](https://twitter.com/adi_12_modi)**
🐱‍💻 **follow me on [github](https://github.com/AditModi)**
✍️ **Do Checkout [my blogs](https://aditmodi.hashnode.dev)** 

Like, share and follow me 🚀 for more content.
Good Luck with your exam! Have Fun. 💪