---
title: "Building Media & Entertainment Predictive
Analytics Solutions on AWS | AWS White Paper Summary"
seoTitle: "Building Media & Entertainment Predictive
Analytics Solutions on AWS |"
seoDescription: "Building Media & Entertainment Predictive
Analytics Solutions on AWS | AWS White Paper Summary"
datePublished: Thu Mar 09 2023 06:24:39 GMT+0000 (Coordinated Universal Time)
cuid: clf0q4vzn00sfj9nv3htc2owg
slug: building-media-entertainment-predictive-analytics-solutions-on-aws-aws-white-paper-summary
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1659099096686/xsILgLhpa.png
tags: aws, data-analytics, media-and-entertainment

---

This whitepaper is intended for data scientists, data architects, and data engineers who want to design and build Media and Entertainment (M&E) predictive analytics solutions on AWS. Specifically, this paper provides an introduction to common cloud-enabled M&E workloads, and describes how a predictive analytics workload fits into the overall M&E workflows in the cloud.
The paper provides an overview of the main phases for the predictive analytics business process, as well as an overview of common M&E predictive analytics use cases. Then, the paper describes the technical reference architecture and tool options for implementing predictive analytics solutions on AWS.

# Introduction

The world of Media and Entertainment (M&E) has shifted from treating customers as mass audiences to forming connections with individuals. This progression was enabled by unlocking insights from data generated through new distribution platforms and web and social networks. M&E companies are aggressively moving from a traditional, mass-broadcasting business model to an Over-The- Top (OTT) model where relevant data can be gathered. In this new model, they are embracing the challenge of acquiring, enriching, and retaining customers through big data and predictive analytics solutions.

As cloud technology adoption becomes mainstream, M&E companies are moving many analytics workloads to AWS to achieve agility, scale, lower cost, rapid innovation, and operational efficiency. As these companies start their journey to the cloud, they have questions about common M&E use cases and how to design, build, and operate these solutions. AWS provides many services in the data and analytics space that are well suited for all M&E analytics workloads, including traditional BI reporting, real-time analytics, and predictive analytics.

In this paper, we discuss the approach to architecture and tools. We’ll cover design, build, and operate aspects of predictive analytics in subsequent papers.

## Overview of AWS Enabled M&E Workloads

M&E content producers have traditionally relied heavily on systems located on- premises for production and post-production workloads. Content producers are increasingly looking into the AWS Cloud to run workloads. This is due to the huge increase in the volume of content from new business models, such as on- demand and other online delivery, as well as new content formats such as 4k and high dynamic range (HDR).
M&E customers deliver live, linear, on-demand, and OTT content with the AWS Cloud. AWS services also enable media partners to build solutions across M&E lines of business. Examples include:
•	Managing digital assets
•	Publishing digital content
•	Automating media supply chains
•	Broadcast master control and play out
 
•	Streamlining content distribution to licensees
•	Affiliates (business to business - B2B)
•	Direct to consumer (business to consumer - B2C) channels
•	Solutions for content and customer analytics using real-time data and machine learning
Figure 1 is a diagram that shows a typical M&E workflow, with a brief description of each area.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ihtbpscqelqztxzb3jhy.png) 
*Figure 1 — Cloud-enabled M&E workflow*

Acquisition — Workloads that capture and ingest media contents such as videos, audio, and images into AWS.
VFX & NLE — Visual Effects (VFX) and nonlinear editing system (NLE) workloads that allow editing of images for visual effects or nondestructive editing of video and audio source files.
DAM & Archive — Digital asset management (DAM) and archive solutions for the management of media assets.
Media Supply Chain — Workloads that manage the process to deliver digital assets such as video or music from the point of origin to the destination.
Publishing — Solutions for media contents publishing.

OTT — Systems that allow the delivery of audio content and video content over the Internet.
 
Playout & Distribution — Systems that support the transmission of media contents and channels into the broadcast network.
Analytics — Solutions that provide business intelligence and predictive analytics capabilities on M&E data. Some typical domain questions to be answered by the analytics solutions are: How do I segment my customers for email campaign?
What videos should I be promoting at the top of audiences OTT/VOD watchlists? Who is at risk of cancelling a subscription? What ads can I target mid-roll to maximize audience engagement? What is the aggregate trending sentiment regarding titles, brands, properties, and talents across social media and where is it headed?

# Overview of the Predictive Analytics Process Flow

There are two main categories of analytics: business and predictive. Business analytics focus on reporting metrics for historical and real-time data. Predictive analytics help predict future events and provide estimations by applying predictive modeling that is based on historical and real-time data. This paper will only cover predictive analytics.
A predictive analytics initiative involves many phases and is a highly iterative process. Figure 2 shows some of the main phases in a predictive analytics project with a brief description of each phase.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zzjh7i8ancu2t66e3rh5.png)
*Figure 2 — Cross-industry standards process for data mining*

1.	Business Understanding — The main objective of this phase is to develop an understanding of the business goals, and then translate the goals into predictive analytics objectives. For the M&E industry, examples of business goals could include increasing content consumption by existing customers, or understanding social sentiment toward contents and talents to assist with new content development. The associated predictive analytics goals could also include personalized content recommendations, and sentiment analysis of social data regarding contents and talents.
2.	Data Understanding — The goal of this phase is to consider the data required for predictive analytics. Initial data collection, exploration, and quality assessment take place during this phase. To develop high-quality models, the dataset needs to be relevant, complete, and large enough to support model training. Model training is the process of training a machine learning model by providing a machine learning algorithm with training data to learn from. Some relevant datasets for M&E use cases are customer information/profile data, content viewing history data, content rating data, social engagement data, customer behavioral data, content subscription data, and purchase data.
 
3.	Data Preparation — Data preparation is a critical step to ensure that high-quality predictive models can be generated. In this phase, the data required for each modeling process is selected. Data acquisition mechanisms need to be created. Data is integrated, formatted, transformed, and enriched for the modeling purpose. Supervised machine learning algorithms require a labeled training dataset to generate predictive models. A labeled training dataset has a target prediction variable and other dependent data attributes or features. The quality of the training data is often considered more important than the machine learning algorithms for performance improvement.
4.	Modeling — In this phase, the appropriate modeling techniques are selected for different modeling and business objectives. For example:
o	A clustering technique could be employed for customer segmentation.
o	A binary classification technique could be used to analyze customer churn.
o	A collaborative filtering technique could be applied to content recommendation.
The performance of the model can be evaluated and tweaked using technical measures such as Area-Under-Curve (AUC) for binary classification (Logistic Regression), Root-Mean-Square (RMSE) for collaborative filtering (Alternating Least Squares), and Sum-of-Squared Error (SSE) for clustering (K-Means). Based on the initial evaluation of the model result, the model settings can be revised and fine-tuned by going back to the data preparation stage.
5.	Model Evaluation — The generated models are formally evaluated in this phase not only in terms of technical measures, but also in the context of the business-success criteria set out during the business- understanding phase. If the model properly addresses the initial business objectives, it can be approved and prepared for deployment.
6.	Deployment — In this phase, the model is deployed into an environment to generate predictions for future events.

# Common M&E Predictive Analytics Use Cases

To a certain extent, some of the predictive analytics use cases for the M&E industry do not differ much from other industries. The following are common use cases that apply to the M&E industry.
Customer segmentation — As the engagement between customers and M&E companies becomes more direct across different channels, and as more data is collected on those engagements, appropriate segmentation of customers becomes increasingly important. Customer relationship management (CRM) strategies, including customer acquisition, customer development, and customer retention, greatly rely upon such segmentation. Although customer segmentation can be achieved using basic business rules, it can only efficiently handle a few attributes and dimensions.  A data-driven segmentation with a predictive modeling approach is more objective and can handle more complex datasets and volumes.
Customer segmentation solutions can be implemented by leveraging clustering algorithms such as k-means, which is a type of unsupervised learning algorithm. A clustering algorithm is used to find natural clusters of customers based on a list of attributes from the raw customer data.
Content recommendation — One of the most widely adopted predictive analytics by M&E companies, this type of analytics is an important technique to maintain customer engagement and increase content consumption. Due to the huge volume of available content, customers need to be guided to the content they might find most interesting. Two common algorithms leveraged in recommendation solutions are content-based filtering and collaborative filtering.
•	Content-based filtering is based on how similar a particular item is to other items, based on usage and rating. The model uses the content attributes of items (categories, tags, descriptions, and other data) to generate a matrix of each item to other items, and calculates similarity based on the ratings provided. Then the most similar items are listed together with a similarity score. Items with the highest score are most similar.
 
•	Collaborative filtering is based on making predictions to find a specific item or user, based on similarity with other items or users. The filter applies weights based on peer user preferences. The assumption is users who display similar profile or behavior have similar preferences for items.
More advanced recommendation solutions can leverage deep learning techniques for better performance. One example of this would be using Recurrent Neural Networks (RNN) with collaborative filtering by predicting the sequence of items in previous streams, such as past purchases.
Sentiment analysis — This is the process of categorizing words, phrases, and other contextual information into subjective feelings. A common outcome for sentiment analysis is positive, negative, or neutral sentiment. Impressions publicized by consumers can be a valuable source of insight into the opinions of broader audiences. These insights, when employed in real time, can be used to significantly enhance audience engagement. Insights can also be used with other analytic learnings such as customer segmentation to identify a positive match between an audience segment and associated content. There are many tools to analyze and identify sentiment, and many of them rely on linguistic analysis that is optimized for a specific context.
From a machine learning perspective, one traditional approach is to consider sentiment analysis as a classification problem. The sentiment of a document, sentence, or word is classified with positive, negative, or neutral labels. In general, the algorithm consists of tokenization of the text, feature extraction, and classification using different classifiers such as linear classifiers (e.g., Support Vector Machine, Logistic Regression) or probabilistic classifiers (e.g., Naïve Bayes, Bayesian Network). However, this traditional approach lacks recognition for the structure and subtleties of written language.
A more advanced approach is to use deep learning algorithms for sentiment analysis. You don’t need to provide these models with predefined features, as the model can learn sophisticated features from the dataset. The words are represented in highly dimensional vectors and features are extracted by the neural network. Examples of deep learning algorithms that can be used for sentiment analysis are Recurrent Neural Network (RNN) and Convolutional Neural Network (CNN). MXNet, Tensorflow, and Caffe are some deep learning frameworks that are well suited for RNN and CNN model training. AWS makes it easy to get started with these frameworks by providing an Amazon Machine Image (AMI) that includes these frameworks preinstalled. This AMI can be run on a large number of instance types, including the P2 instances that provide general 
purpose GPU processing for deep learning applications. The Deep Learning AMI is available in the AWS Marketplace.
Churn prediction — This is the identification of customers who are at risk of no longer being customers. Churn prediction helps to identify where to deploy retention resources most effectively. The data used in churn prediction is generally user activity data related to a specific service or content offering. This type of analysis is generally solved using a logistic regression with a binary classification. The binary classification is designated as customer leave predicted or customer retention predicted. Weightings and cutoff values can be used with predictive models to tweak the sensitivity of predictions to minimize false positives or false negatives to optimize for business objectives. For example Amazon Machine Learning (Amazon ML) has an input for cutoff and sliders for precision, recall, false positive rate, and accuracy.

# Predictive Analytics Architecture on AWS

AWS includes the components needed to enable pipelines for predictive analytics workflows. There are many viable architectural patterns to effectively compute predictive analytics. In this section, we discuss some of the technology options for building predictive analytics architecture on AWS. Figure 3 shows one such conceptual architecture.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/qdf1yi4h6aqzck8rw6nl.png)
*Figure 3 — Conceptual reference architecture*

More Details can be found [here](https://d1.awsstatic.com/whitepapers/Analytics/ME%20Advanced%20Analytics%20on%20AWS.pdf?did=wp_card&trk=wp_card). 

# AWS Services and Benefits

Machine learning solutions come in many shapes and sizes. Some of the AWS services commonly used to build machine learning solutions are described in the following sections. During the predictive analytics process workflow, different resources are needed throughout different parts of the lifecycle. AWS services work well in this scenario because resources can run on demand and you pay only for the services you consume. Once you stop using them, there are no additional costs or termination fees.

More Details can be found [here](https://d1.awsstatic.com/whitepapers/Analytics/ME%20Advanced%20Analytics%20on%20AWS.pdf?did=wp_card&trk=wp_card).

# Conclusion

In this paper, we provided an overview of the common Media and Entertainment (M&E) predictive analytics use cases. We presented an architecture that uses a broad set of services and capabilities of the AWS Cloud to enable both the data scientist workflow and the predictive analytics generation workflow in production.

# Reference

[Original paper](https://d1.awsstatic.com/whitepapers/Analytics/ME%20Advanced%20Analytics%20on%20AWS.pdf?did=wp_card&trk=wp_card)

[IMPORTANT]
- This material is not mine. This material was written exclusively by Amazon Web Services.

I am sharing this with a bigger IT audience to help folks in their quest to learn AWS. I don't intend to monetize anything, therefore everything I share will always be free. I'm not interested in profiting off the knowledge of others.

Sharing relevant and helpful content is something I've always thought is a great approach to support the tech community. I'll keep offering my viewers insightful content. Thank you for supporting.