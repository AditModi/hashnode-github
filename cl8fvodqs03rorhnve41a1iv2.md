## Top 5 Sessions at KubeCon + CloudNativeCon EU

# What is KubeCon + CloudNativeCon EU ?


👉 The Cloud Native Computing Foundation’s flagship conference gathers adopters and technologists from leading open source and cloud native communities.


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ok8yksmp34zgnq1mfa3o.JPEG)

👉 KubeCon + CloudNativeCon is the conference for gathering developers, IT professionals, and C-level leaders across the ecosystem to share learnings, highlight innovation, and discuss the future of cloud native computing, including emerging trends in microservices architectures and container orchestration with technologies like Kubernetes, Prometheus, and many more.



👉 Over 7,000 of the most talented individuals in the industry gather together for this event making it a great place to network with industry professionals and learn from the top cloud native experts. 

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/eksl973it2rh55tzxuic.png)

> KubeCon + CloudNativeCon provides an important forum for exchanging relevant information and insights on Kubernetes and broader DevOps trends.

- For 67% of attendees, it is their first KubeCon making it a great place for newcomers to the cloud native community to learn.
- The top two reasons that people attend KubeCon + CloudNativeCon are for career growth/training (43%) and networking (30%).
- 24% of attendees are developers with more than 73% in technical positions overall.
- 7,290 companies participated, with the majority from Information Technology (67%) and Financial (11%) industries.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/n7x9nxyaf13n20vzij1j.JPEG)

# My Experience of Attending the KubeCon + CloudNativeCon EU 2022 (In-person) for the first time

👉 I was given the opportunity to attend my first-ever in-person KubeCon + CloudNativeCon this year in Valencia, Spain, thanks to the CNCF and Linux Foundation for the Dan Kohn Scholarship. I'm grateful that the Linux Foundation and Cloud-Native Computing Foundation allowed me to attend the event.

👉 I have shared the complete blog post [here](https://dev.to/aditmodi/my-experience-of-attending-the-kubecon-cloudnativecon-2022-in-person-for-the-first-time-23id-temp-slug-6909938?preview=c1f5cf719628e5a39bccbd4df3ffdc3963d1b817b9a1e5a77737213e04010e54c95f292bcc72746f811348a7316d0744f4aa2f76d299c7421d53c1c5).

# Top 5 Sessions at KubeCon + CloudNativeCon EU

👉 Attending the event was a rewarding experience; listed below are the sessions I found most interesting and returned to several times.

(Click links to skip to section)

1. [Tower of Babel: Making Apache Spark, Kubeflow, and Kubernetes Play Nice - Holden Karau, Netflix](#SESSION)
2. [Spark on Kubernetes: The Elastic Story - Bowen Li & Huichao Zhao, Apple](#SESSION)
3. [Why Kubernetes Can't Get Around FinOps – Cost Management Best Practice - Vanessa Kantner & Manuela Latz, Liquid Reply](#SESSION)
4. [From Kubernetes to PaaS to … Err, What’s Next? - Daniel Bryant, Ambassador Labs](#SESSION)
5. [Keynote: 7 Years of Running Kubernetes for Mercedes-Benz - Jens Erat, DevOps Engineer; Peter Mueller, Lead Expert; Sabine Wolz, Product Owner, Mercedes-Benz Tech Innovation](#KEYNOTE)

## 1. Tower of Babel: Making Apache Spark, Kubeflow, and Kubernetes Play Nice - Holden Karau, Netflix - SESSION

> Working with big data matrices is challenging, Kubernetes allows users to elastically scale, but can only have a pod as large as a node, which may not be large enough to fit the matrix in memory. While Kubernetes allows for other paradigms on top of it which allows pods to coordinate on individual jobs, setting them up and making them play nice with ML platforms is not straightforward. 

👉 Using Apache Spark and Apache Mahout we can work with matrices of any dimension and distribute them across an unbounded number of pods/nodes, and we can use Kubeflow to make our work quickly and easily reproducible. 

👉 In this talk, the authors will discuss how they used Apache Spark and Mahout to denoise DICOM images of lungs of COVID patients and published their Pipeline with Kubeflow to make the process easily repeatable which could help doctors in more resource limited hospitals, as well as other researchers seeking to automate the detection of COVID.

{% embed https://www.youtube.com/watch?v=6eLkjPvzKRs %}

## 2. Spark on Kubernetes: The Elastic Story - Bowen Li & Huichao Zhao, Apple - SESSION

> Apache Spark is a unified analytics engine for large-scale data processing. People are moving Spark and batch workload to Kubernetes due to its uprising popularity. There are many challenges to running Spark efficiently on Kubernetes, for example, supporting autoscaling-based workloads. 

👉 In this talk, the authors discuss building a large scale Spark Service on top of Kubernetes. They will also walk through autoscaling on a multi-tenant platform with advanced features such as physical isolation, min/max capacity setting, bin-packing, scale-in and scale out controls, and more. These improvements show significant CPU and memory utilization savings for Spark on Kubernetes.

{% embed https://www.youtube.com/watch?v=n7WeoTJq-40 %}

## 3. Why Kubernetes Can't Get Around FinOps – Cost Management Best Practice - Vanessa Kantner & Manuela Latz, Liquid Reply - SESSION

> 
Anyone with the right permissions on a cloud provider can acquire resources or spin up Kubernetes Clusters. While developers can joyfully make cloud spending explode, traditional finance and procurement departments look around in wonder. 

👉 The FinOps approach and the Foundation, which coined the word, dedicate itself to continuously enhancing best practices around cloud financial management. Managing Kubernetes resources is the masterclass of it. Having cost transparency and control over many dynamically scaling containers across many server instances can be difficult. 

👉 Vanessa and Manuela share the experience in monitoring Kubernetes costs and planning budgets accordingly. This session covers how engineers – responsible for incurring costs – can support cloud cost management to prevent overspending and how this approach enables and empowers colleagues from finance, procurement and business in their daily doing. This, in turn, gives the engineer more freedom to explore new solutions.

{% embed https://www.youtube.com/watch?v=zqJ9CqaQpYw %}

## 4. From Kubernetes to PaaS to … Err, What’s Next? - Daniel Bryant, Ambassador Labs - SESSION

> Developers building applications on Kubernetes today are being asked to not just code applications -- they are also responsible for shipping and running their applications, too. We often talk about needing a Kubernetes platform, but are we really looking for a PaaS? Or instead, are we looking for some kind of developer control plane with a Goldilock-sized collection of tools that provides just the right amount of platform? 

👉 This talk will look back on my experience of building platforms, both as an end-user and now as part of an organization helping our customers do the same. The key takeaways are: - Treat platform as a product - Realize that you can’t have good developer experience (DevEx) without good UX - Focus on workflows and tooling interoperability 

👉 We’ll wrap this talk with a walk-through of the CNCF ecosystem through the developer control plane lens, and look at what’s next in the future of this important emerging category.

{% embed https://www.youtube.com/watch?v=btUYeOa7JPI %}

## 5. Keynote: 7 Years of Running Kubernetes for Mercedes-Benz - Jens Erat, DevOps Engineer; Peter Mueller, Lead Expert; Sabine Wolz, Product Owner, Mercedes-Benz Tech Innovation - KEYNOTE

> Years ago, software engineers faced hard times at Mercedes-Benz: spreadsheet operations, manual processes, grown infrastructure and strict governance. A grassroots initiative of engineers accepted the challenge to change the game – and their silver bullet was Kubernetes. 

👉 Join us on our journey from introducing Kubernetes 0.9 on managed servers to an on-premises self-service cloud platform with close to 1000 clusters on Cluster API. 

👉 You will learn about our stake transforming a data center with a young team that mostly did not know enterprise processes before. We describe how mixing naive visions and a strong believe in open source with lots of resilience made the project a success.

{% embed https://www.youtube.com/watch?v=UmbjwSK9b3I %}

# Conclusion

Through this guide I have shared my top 5 sessions after attending my first KubeCon + CloudNativeCon Event (in-person). It was a memorable experience and I am extremely grateful to CNCF for this opportunity.

Let me know your thoughts in the comment section 👇
And if you haven't yet, make sure to follow me on below handles:

👋 **connect with me on [LinkedIn](https://www.linkedin.com/in/adit-modi-2a4362191/)**
🤓 **connect with me on [Twitter](https://twitter.com/adi_12_modi)**
🐱‍💻 **follow me on [github](https://github.com/AditModi)**
✍️ **Do Checkout [my blogs](https://aditmodi.hashnode.dev)** 

Like, share and follow me 🚀 for more content.
