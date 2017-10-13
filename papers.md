# Papers citing Google study

Prevalent themes: heterogeneity, failure analysis, heavy-tail in task resource
consumption, ...

* [Scheduling papers](#scheduling-papers)
* [Heavy-tailed distributions](#heavy-tailed-distributions)
* [High-variability workload](#high-variability-workload)
* [Failure studies](#failure-studies)
* [Heterogeneity and low utilization](#heterogeneity-and-low-utilization)
* [Comparable work](#comparable-work)
* [Other papers](#other-papers)

## Scheduling papers

The following papers propose new scheduling algorithms that are based on, or
evaluated using the Google trace. We should study them carefully to see if
their assumptions hold for non-Google traces.

* M Schwarzkopf, A Konwinski, M Abd-El-Malek et al.
  "Omega: flexible, scalable schedulers for large compute clusters",
  EuroSys 2013 (298 citations).
  - Based on the assumption of workload heterogeneity (in terms of job
    requirements and hardware), so it makes sense to use multiple schedulers.
  - "Most (>80%) jobs are batch jobs, but the majority of resources (55-80%)
    are allocated to service jobs (Figure 2); the latter typically run for much
    longer (Figure 3), and have fewer tasks than batch jobs (Figure 4). [This
    matters because] many batch jobs are short and fast turnaround is important,
    so a lightweight, low-quality approach to placement works just fine. But
    long-running, high-priority service jobs (20-40% of them run for over a
    month) must meet stringent availability and performance targets, meaning
    that careful placement of their tasks is needed to maximize resistance to
    failures and provide good performance."
  - Scheduler decision time is modeled as a linear function, assuming that
    the cost to schedule each task is the same. This is reasonable for Google
    because most real-life workloads have tasks with identical requirements

* A Verma, L Pedrosa, M Korupolu et al.
  "Large-scale cluster management at Google with Borg",
  EuroSys 2015 (237 citations).
  - Uses study to explain why Borg aims for fine-grained resource requests:
    because users typically request CPU in units of milli-cores

* CC Hung, L Golubchik, M Yu.
  "Scheduling jobs across geo-distributed datacenters",
  SoCC 2015 (25 citations).
  - Uses trace for evaluation
  - SWAG scheduling algorithm may have high computational overhead (so low
    utilization of resources is expected?)

* X Xu, H Yu.
  "A game theory approach to fair and efficient resource allocation in cloud computing",
  Mathematical Problems in Engineering 2014 (21 citations).
  - Uses Google trace in evaluation; algorithm utilized 10+% more resources
    than requested by the user

* S Kanev, K Hazelwood, GY Wei et al.
  "Tradeoffs between power management and tail latency in warehouse-scale applications",
  IISWC 2014 (21 citations).
  - In a shared cluster, jobs that get co-scheduled together may often have
    over-provisioned performance requirements. Thus a thread-granular
    scheduler becomes important to utilize resources efficiently

* C Chen, W Wang, S Zhang, B Li.
  "Cluster Fair Queueing: Speeding up Data-Parallel Jobs with Delay Guarantees",
  INFOCOM 2017 (NEW).
  - Schedules shortest jobs first, but assumes it can perfectly predict runtime
    and simulations only consider up to 20% prediction error
  - Uses Google trace in evaluation

* C Delimitrou, C Kozyrakis.
  "Hcloud: Resource-efficient provisioning in shared cloud systems",
  ASPLOS 2016 (15 citations).
  - We show that to achieve reasonable performance predictability, it is crucial
    to understand the resource preferences and sensitivity to interference of
    individual applications
  - End-users having to specify how many resources each job should use is
    error-prone and leads to resource overprovisioning

* IC Gog, M Schwarzkopf, A Gleave, RNM Watson et al.
  "Firmament: Fast, centralized cluster scheduling at scale",
  USENIX OSDI 2016 (7 citations).
  - Demonstration of high placement latency of Quincy, using the Google trace

* W Wang, B Li, B Liang, J Li.
  "Multi-resource fair sharing for datacenter jobs with placement constraints",
  SC 2016 (3 citations).
  - Heterogeneity of hardware and job requests
  - Assumes same demand vector across a user's tasks



## Heavy-tailed distributions

The following papers assume heavy-tailed distributions to describe job duration
and/or resource requests. They leverage this characteristic to resolve issues
like stragglers, by allowing short jobs (which are assumed to be the majority)
run multiple times.

* G Ananthanarayanan, A Ghodsi, S Shenker, I Stoica.
  "Effective Straggler Mitigation: Attack of the Clones",
  NSDI 2013 (160 citations).
  - Mitigates stragglers by duplicating jobs. Study is used to argue that jobs
    in the cloud follow a heavy-tailed distribution, so it's ok to duplicate
    them. For Facebook and Bing, 90% of smallest jobs consume less than 10% of
    the overall cluster cycles. For Google specifically, 92% of jobs account
    for only 2% of overall resources).

* P Delgado, F Dinu, AM Kermarrec et al.
  "Hawk: Hybrid datacenter scheduling",
  ATC 2015 (35 citations).
  - Uses trace-driven simulations based on the Google trace
  - Uses reference to support idea of heterogeneity in datacenters
  - Trace is analyzed to show heavy-tailed distribution of resource usage
    among tasks, which justifies dedicating a small part of the datacenter
    for scheduling small tasks (they do this to avoid head-of-line blocking)
  - Note: this fraction of the datacenter will likely have to be
    much larger than the 17% evaluated in the paper. We have to confirm that.

* M Carvalho, W Cirne, F Brasileiro, J Wilkes.
  "Long-term slos for reclaimed cloud computing resources",
  SoCC 2014 (28 citations).
  - Datacenter utilization is low
  - There are services that stay up for weeks or months at a time
  - Heterogeneity and dynamicity of workload is such that the coarse-grain
    estimations of available capacity in the paper may be off

* P Thinakaran, JR Gunasekaran, B Sharma et al.
  "Phoenix: A Constraint-aware Scheduler for Heterogeneous Datacenters",
  ICDCS 2017 (NEW).
  - Flexibility of task migration for short-lived interactive applications
    which dominate (80-90%) the datacenter. Uses constraints in Google paper
    to decide whether migration is possible.
  - Note: The paper never defines *clearly* where to draw the line
    between short and long jobs. In any case, jobs in our traces are much
    longer than in the Google trace. Unfortunately we don't have any information
    on job constraints for our traces.

* P Delgado, D Didona, F Dinu et al.
  "Job-aware scheduling in eagle: Divide and stick to your probes",
  SoCC 2016 (4 citations).
  - Majority of jobs are short, but consume only limited resources, but a small
    number of jobs consume a large amount of resources
  - Uses trace to evaluate approach, estimates length of job/tasks using average
    of task duration from previous job runs.

* S Rampersaud, D Grosu.
  "Sharing-Aware Online Virtual Machine Packing in Heterogeneous Resource Clouds",
  IEEE Transactions on Parallel and Distributed Systems 2017 (NEW).
  - Resource requests follow a heavy-tailed distribution

* S Chen, M Ghorbani, Y Wang et al.
  "Trace-based analysis and prediction of cloud computing user behavior using the fractal modeling technique",
  IEEE BigData 2014 (13 citations).
  - Heavy-tailed distributions for job duration and resource requests per job
  - Heterogeneity and dynamicity suggesting that Poisson arrival rate and
    Gaussian distribution for task duration are not applicable

* J Liu, H Shen, L Chen.
  "CORP: Cooperative opportunistic resource provisioning for short-lived jobs in cloud systems",
  IEEE CLUSTER 2016 (9 citations).
  - Low overall utilization, majority of jobs are short


## High-variability workload

The following 6 studies assume high variability in the workload.

* F Xu, F Liu, H Jin, AV Vasilakos.
  "Managing performance overhead of virtual machines in cloud computing: A survey, state of the art, and future directions",
  Proceedings of the IEEE, 2014 (131 citations).
  - Uses the study to claim that computing resource demands are highly varying
    over short time intervals, which makes prediction of resource demands hard,
    and a lot of VM performance management solutions inapplicable.

* I Narayanan, A Kansal, A Sivasubramaniam.
  "Right-sizing Geo-distributed Data Centers for Availability and Latency",
  ICDCS 2017 (NEW).
  - Diurnal patterns are expected

* X Chang, B Wang, JK Muppala et al.
  "Modeling active virtual machines on IaaS clouds using an M/G/m/m+ K queue",
  IEEE Transactions on Services Computing 2016 (5 citations).
  - CoV > 1 for job service time distribution

* L Zheng, C Joe-Wong, CG Brinton, CW Tan et al.
  "On the Viability of a Cloud Virtual Service Provider",
  SIGMETRICS 2016 (4 citations).
  - Localized instances of high demand and high user waiting times
  - Uses trace for evaluation

* A Barbalace, R Lyerly, C Jelesnianski et al.
  "Breaking the Boundaries in Heterogeneous-ISA Datacenters",
  ASPLOS 2017 (2 citations).
  - Assumes execution times range from milliseconds to hundreds of seconds
  - Goal is to migrate containers between ISAs
  - Note: Not sure whether the existence of longer execution times
    would weaken their evaluation. The question is rather: how deep are the
    stacks that must be converted? They use the NPB benchmarks.

* J Ma, X Sui, N Sun, Y Li, Z Yu, B Huang, T Xu et al.
  "Supporting differentiated services in computers via programmable architecture for resourcing-on-demand (pard)",
  ASPLOS 2015 (7 citations).
  - Developers exaggerate resource requirements
  - Performance variability because most jobs are very short (few minutes)
  - Note: Not sure what *most* or *short* means, but we are not sure
    whether performance variability exists because of shorter jobs. Must
    investigate.



## Failure studies

The following 5 studies analyze failures in the Google trace.

* X Chen, CD Lu, K Pattabiraman.
  "Failure analysis of jobs in compute clouds: A google cluster case study",
  ISSRE 2014 (28 citations).
  - Analyzes Google trace to characterize job failures. Concludes that there are
    many opportunities to enhance the reliability of cloud applications, e.g.
    proactive maintenance of nodes, or limiting job resubmission
  - Also finds that resource usage patterns of jobs can be predictive of failure

* P Garraghan, IS Moreno, P Townend et al.
  "An analysis of failure-related energy waste in a large-scale cloud environment",
  IEEE Transactions on Emerging Topics in Computing 2014 (20 citations).
  - Analysis of failures in Google trace
  - 88% of task failure events occur in lower priority tasks producing 13% of
    total energy waste, and 1% of failure events occur in higher priority
    tasks incurring 8% of energy waste.

* B Ghit, D Epema.
  "Better Safe than Sorry: Grappling with Failures of In-Memory Data Analytics Frameworks",
  HPDC 2017 (NEW).
  - Cites very large failure rates in the trace: 13 machines/hour, 400 tasks/hr
  - Concludes that these failures will result in wasted work
  - Finds linear relationship between number of tasks in job and median CPU
    seconds wasted by the job

* A Rosa, LY Chen, W Binder.
  "Predicting and mitigating jobs failures in big data clusters",
  CCGrid 2015 (14 citations).
  - Attempts to predict job failures in the Google trace using several
    well-known techniques
  - Job arrivals exhibit strong time-variability (making it hard to predict
    outcome)

* N El-Sayed, H Zhu, B Schroeder.
  "Learning from failure across multiple clusters: A trace-driven approach to understanding, predicting, and mitigating job terminations",
  ICDCS 2017 (NEW).
  - Analyzes job terminations in the Google, past LANL, and CMU OpenCloud traces



## Comparable work

These are studies that compare a different trace with the Google trace.

* S Shen, V van Beek, A Iosup.
  "Statistical characterization of business-critical workloads hosted in cloud datacenters",
  CCGrid 2015 (14 citations).
  - They study private clouds and compare them to the Google study. They have
    released the data in the Grid Workloads Archive
  - Cited as a workload that does not match the profile of business-critical
    applications. CPU/memory workloads change less frequently than for
    business-critical applications.

* I Cano, S Aiyar, A Krishnamurthy.
  "Characterizing private clouds: A large-scale empirical analysis of enterprise clusters",
  SoCC 2016 (2 citations).
  - More homogeneous


## Heterogeneity and low utilization

The 21 papers that follow cite the Google study as proof that modern datacenters
are expected to be heterogeneous and underutilized. In the case of other HPC
traces, utilization can range anywhere from 10.7-87.9%
(http://www.cs.huji.ac.il/labs/parallel/workload/logs.html)

* J Li, JF Naughton, RV Nehme.
  "Resource bricolage and resource selection for parallel database systems",
  VLDB 2017 (NEW).
  - 2-8x differences in machine performance in the same cluster should be
    expected, so coming up with the right partitioning strategy is important

* J Gu, Y Lee, Y Zhang, M Chowdhury, KG Shin.
  "Efficient Memory Disaggregation with Infiniswap",
  NSDI 2017 (NEW).
  - "Resources are often underutilized" and they assume that there are more idle
    CPUs (e.g., 40%) than idle memory (e.g. 30%) in the cluster (Infiniswap's
    benefits are limited by the latter).
  - "Rightsizing is difficult because applications often overestimate their
    requirements"
  - Considers a container-based model

* C Delimitrou, C Kozyrakis.
  "Quasar: resource-efficient and QoS-aware cluster management",
  ASPLOS 2014 (225 citations).
  - Uses their own Twitter data and the study to argue that cloud facilities
    operate at very low utilization. It is argued that this is because it is
    difficult for cluster management software to effectively utilize available
    resources, and for users to accurately estimate their needs. A cluster
    management approach is proposed to better utilize available resources.

* Q Zhang, MF Zhani, R Boutaba et al.
  "Dynamic heterogeneity-aware resource provisioning in the cloud",
  IEEE Transactions on Cloud Computing, 2014 (79 citations).
  - Uses study as motivation that modern clusters are heterogeneous

* J Leverich, C Kozyrakis.
  "Reconciling high server utilization and sub-millisecond quality-of-service",
  EuroSys 2014 (76 citations).
  - Uses study as motivation to argue that server utilization is typically low
    in the cloud, so underutilized resources (and their availability for other
    tasks) can be assumed. We need to be careful that they mean *actual*
    utilization

* D Lo, L Cheng, R Govindaraju et al.
  "Heracles: improving resource efficiency at scale",
  ISCA 2015 (67 citations).
  - Uses reference to support point that utilization of most datacenters is low

* W Wang, B Li, B Liang.
  "Dominant resource fairness in cloud computing systems with heterogeneous servers",
  INFOCOM 2014 (66 citations).
  - Uses reference to support idea that modern datacenters sport heterogeneous
    hardware

* Q Zhang, MF Zhani, R Boutaba et al.
  "Harmony: Dynamic heterogeneity-aware resource provisioning in the cloud",
  ICDCS 2013 (65 citations).
  - Uses reference to support idea that modern datacenters sport heterogeneous
    hardware

* AA Bhattacharya, D Culler, E Friedman et al.
  "Hierarchical scheduling for diverse datacenter workloads",
  SoCC 2013 (65 citations).
  - Uses reference to support idea that datacenter workloads are diverse

* Y Wang, R Chen, DC Wang.
  "A survey of mobile cloud computing applications: perspectives and challenges",
  Wireless Personal Communications, 2015 (49 citations).
  - Uses reference to support idea of heterogeneity in workloads

* W Wang, B Liang, B Li.
  "Multi-resource fair allocation in heterogeneous cloud computing systems",
  IEEE Transactions on Parallel and Distributed Systems, 2015 (31 citations).
  - Uses reference to support idea of heterogeneity

* R Gandhi, D Xie, YC Hu.
  "PIKACHU: How to Rebalance Load in Optimizing MapReduce On Heterogeneous Clusters",
  ATC 2013 (27 citations).
  - Heterogeneity in hardware and workload (foreground vs background)

* H He, J Hu, D Da Silva.
  "Enhancing Datacenter Resource Management through Temporal Logic Constraints",
  IPDPS 2017 (NEW).
  - Servers in a datacenter have a very diverse power profile due to hardware
    heterogeneity, workload fluctuation, and task placement constraints

* Z Dong, N Liu, R Rojas-Cessa.
  "Greedy scheduling of tasks with time constraints for energy-efficient cloud-computing data centers",
  Journal of Cloud Computing 2015 (17 citations).
  - Datacenter workloads are heterogeneous

* A Tumanov, T Zhu, JW Park, MA Kozuch et al.
  "TetriSched: global rescheduling with adaptive plan-ahead in dynamic heterogeneous clusters",
  EuroSys 2016 (14 citations).
  - Heterogeneity of cluster resources
  - Doesn't use trace in evaluation

* T Heinze, L Roediger, A Meister, Y Ji, Z Jerzak et al.
  "Online parameter optimization for elastic data stream processing",
  SoCC 2015 (11 citations).
  - Actual utilization of servers is low

* E Pettijohn, Y Guo, P Lama, X Zhou.
  "User-Centric Heterogeneity-Aware MapReduce Job Provisioning in the Public Cloud",
  ICAC 2014 (11 citations).
  - Cluster heterogeneity

* Y Yang, GW Kim, WW Song, Y Lee, A Chung et al.
  "Pado: A data processing engine for harnessing transient resources in datacenters",
  EuroSys 2017 (2 citations).
  - Datacenter resources under-utilized

* A Harlap, H Cui, W Dai, J Wei, GR Ganger, PB Gibbons et al.
  "Addressing the straggler problem for iterative convergent parallel ML",
  ACM SoCC 2016 (3 citations).
  - Hardware heterogeneity

* M Nguyen, Z Li, F Duan, H Che, H Jiang.
  "The tail at scale: how to predict it?",
  USENIX HotCloud 2016 (NEW).
  - Current practice: overprovision datacenter resources to meet SLO at the
    cost of low resource utilization (\<50% CPU and memory utilization)

* A Rosa, LY Chen, W Binder.
  "Understanding the dark side of big data clusters: an analysis beyond failures",
  IEEE DSN 2015 (12 citations).
  - Analyzes dependencies between failed tasks/jobs
  - Workload heterogeneity, low server utilization



## Other papers

* X Zhang, E Tune, R Hagmann, R Jnagal et al.
  "CPI^2: CPU performance isolation for shared compute clusters",
  EuroSys 2013 (123 citations).
  - Uses study as motivation for the existence of job classes that must be
    isolated to avoid performance interference

* A Basu, J Gandhi, J Chang, MD Hill et al.
  "Efficient virtual memory for big memory servers",
  ISCA 2013 (117 citations).
  - Uses paper to assumes that memory usage changes little over a job's runtime

* D Çavdar, A Rosà, LY Chen, W Binder et al.
  "Quantifying the brown side of priority schedulers: Lessons from big clusters",
  ACM SIGMETRICS 2014 (11 citations).
  - Analysis mainly based on job priority


Papers that casually reference Google study, without actually using the results:

* B Jennings, R Stadler.
  "Resource management in clouds: Survey and research challenges",
  Journal of Network and Systems Management, 2014 (174 citations).
  - Unused reference

* H Li, A Ghodsi, M Zaharia, S Shenker et al.
  "Tachyon: Reliable, memory speed storage for cluster computing frameworks",
  SoCC 2014 (130 citations).
  - Incorrectly referenced

* C Klein, M Maggio, KE Årzén et al.
  "Brownout: Building more robust cloud applications",
  ICSE 2014 (70 citations).
  - Used to argue that cloud applications have dynamic loads and veriable number
    of users, and thus dynamic resource capacity requirements.

* H Xu, C Feng, B Li.
  "Temperature aware workload management in geo-distributed datacenters",
  ICAC 2013 (69 citations).
  - Simple reference to point out the existence of mixed workloads in the cloud

* ZÁ Mann.
  "Allocation of virtual machines in cloud data centers—a survey of problem models and optimization algorithms",
  ACM Computing Surveys 2015 (68 citations).
  - Simple reference to point out existence of traces

* Z Ren, J Wan, W Shi, X Xu et al.
  "Workload analysis, implications, and optimization on a production hadoop cluster: A case study on taobao",
  IEEE Transactions on Services Computing, 2014 (41 citations).
  - Uses reference as example of another type of workload analysis study

* DG Feitelson, D Tsafrir, D Krakov.
  "Experience with using the parallel workloads archive",
  J. Parallel Distrib. Comput. 2014 (40 citations).
  - Quotes trace format

* T Xu, Y Zhou.
  "Systems approaches to tackling configuration errors: A survey",
  ACM Computing Surveys (CSUR) 2015 (25 citations).
  - Mis-referenced

* AA Eldin, A Rezaie, A Mehta, S Razroev et al.
  "How will your workload look like in 6 years? analyzing wikimedia's workload",
  IC2E 2014 (20 citations).
  - Simply cited as another workload analysis paper

* MR Mesbahi, AM Rahmani et al.
  "Cloud dependability analysis: Characterizing Google cluster infrastructure reliability",
  ICWR 2017 (NEW).
  - Fits a continuous Markov model to failure data

* A Albatli, D McKee, P Townend, L Lau, J Xu.
  "PROV-TE: A Provenance-Driven Diagnostic Framework for Task Eviction in Data Centers",
  IEEE BigData Service and Applications 2017 (NEW).

* C Chen, W Wang, B Li.
  "Speculative Slot Reservation: Enforcing Service Isolation for Dependent Data-Parallel Computations",
  ICDCS 2017 (NEW).

* O Adam, YC Lee, AY Zomaya.
  "Stochastic Resource Provisioning for Containerized Multi-Tier Web Services in Clouds",
  IEEE Transactions on Parallel and Distributed Systems 2017 (NEW).
  - Very generic reference

* C Wang, B Urgaonkar, N Nasiriani et al.
  "Using Burstable Instances in the Public Cloud: Why, When and How?",
  ACM Meas. Anal. Comput. Syst. 2017 (NEW).
  - Very generic reference

* P Garraghan, P Townend, J Xu.
  "An empirical failure-analysis of a large-scale cloud computing environment",
  High-Assurance Systems 2014 (19 citations).

* C Wang, N Nasiriani, G Kesidis, B Urgaonkar et al.
  "Recouping energy costs from cloud tenants: Tenant demand response aware pricing design",
  e-Energy 2015 (19 citations).
  - Workloads can be "complex" ?!

* H AlJahdali, A Albatli, P Garraghan et al.
  "Multi-tenancy in cloud computing",
  IEEE SOSE 2014 (16 citations).
  - Not actually referenced (!)

* X Chen, CP Ho, R Osman, PG Harrison et al.
  "Understanding, modelling, and improving the performance of web applications in multicore virtualised environments",
  ICPE 2014 (15 citations).
  - Ambiguous citation (argues that the proposed approach achieves lower power
    consumption than Google's datacenter)

* M Schwarzkopf, MP Grosvenor, S Hand.
  "New wine in old skins: the case for distributed operating systems in the data center",
  Proceedings of the 4th Asia 2013 (12 citations).
  - Simple citation for the size of the datacenter

* A Ali-Eldin, M Kihl, J Tordsson, E Elmroth.
  "Analysis and characterization of a video-on-demand service workload",
  Proceedings of the 6th ACM 2015, (12 citations).
  - Shallow reference

* D Cheng, J Rao, C Jiang, X Zhou.
  "Resource and deadline-aware job scheduling in dynamic hadoop clusters",
  IPDPS 2015 (11 citations).
  - Cites dynamicity but doesn't use trace in evaluation

* MC Calzarossa, L Massari, D Tessera.
  "Workload characterization: A survey revisited",
  ACM Computing Surveys (CSUR 2016 (11 citations).
  - Simple survey

* B Zong, R Raghavendra, M Srivatsa et al.
  "Cloud service placement via subgraph matching",
  IEEE ICDE 2014 (10 citations).

* M Ghorbani, Y Wang, Y Xue, M Pedram et al.
  "Prediction and control of bursty cloud workloads: a fractal framework",
  ESWEEK 2014 (10 citations).

* OA Abdul-Rahman, K Aida.
  "Towards understanding the usage behavior of Google cloud users: the mice and elephants phenomenon",
  ICCCTS 2014 (10 citations).

* Q Zhang, R Boutaba.
  "Dynamic workload management in heterogeneous cloud computing environments",
  Network Operations and Management 2014 (10 citations).

* G Kesidis, Y Shan, B Urgaonkar et al.
  "Network calculus for parallel processing",
  ACM SIGMETRICS 2015 (8 citations).


Other other papers

* A Tumanov, J Wise, O Mutlu, GR Ganger.
  "Asymmetry-aware execution placement on manycore chips",
  SFMA 2013 (7 citations).

* JO Iglesias, L Murphy, M De Cauwer et al.
  "A methodology for online consolidation of tasks through more accurate resource estimations",
  Utility and Cloud 2014 (5 citations).

* S Spicuglia, LY Chen, W Binder.
  "Join the best queue: Reducing performance variability in heterogeneous systems",
  IEEE CLOUD 2013 (7 citations).

* B Nicolae, MM Rafique.
  "Leveraging collaborative content exchange for on-demand VM multi-deployments in iaas clouds",
  European Conference on Parallel Processing 2013 (8 citations).

* MAS Netto, MD Assunçao et al.
  "Leveraging attention scarcity to improve the overall user experience of cloud services",
  Network and Service 2013 (4 citations).

* C Klein, M Maggio, KE Årzén et al.
  "Introducing service-level awareness in the cloud",
  Proceedings of the 4th 2013 (4 citations).

* T Wang, H Xu, F Liu.
  "Multi-Resource Load Balancing for Virtual Network Functions",
  ICDCS 2017 (2 citations).

* L Wei, CH Foh, B He, J Cai.
  "Towards efficient resource allocation for heterogeneous workloads in IaaS clouds",
  IEEE Transactions on Cloud 2015 (3 citations).

* R Han, S Zhan, C Shao, J Wang, LK John, J Xu et al.
  "Bigdatabench-mt: A benchmark tool for generating realistic mixed data center workloads",
  IEEE BigData 2015 (5 citations).

* Z Ren, W Shi, J Wan.
  "Towards realistic benchmarking for cloud file systems: Early experiences",
  IEEE IISWC 2014 (4 citations).

* L Jehl, R Vitenberg, H Meling.
  "Smartmerge: A new approach to reconfiguration for atomic storage",
  International Symposium on Distributed 2015 (6 citations).

* DC Juan, L Li, HK Peng, D Marculescu et al.
  "Beyond poisson: Modeling inter-arrival time of requests in a datacenter",
  Pacific-Asia Conference 2014 (11 citations).

* J Patel, V Jindal, IL Yen, F Bastani, J Xu et al.
  "Workload estimation for improving resource management decisions in the cloud",
  IEEE ISADS 2015 (7 citations).

* SF Piraghaj, RN Calheiros, J Chan et al.
  "Virtual machine customization and task mapping architecture for efficient allocation of cloud data center resources",
  The Computer 2015 (5 citations).

* P Markthub, A Nomura et al.
  "Using rCUDA to Reduce GPU Resource-Assignment Fragmentation Caused by Job Scheduler",
  Parallel and Distributed 2014 (4 citations).

* P Garraghan, D McKee, X Ouyang et al.
  "Seed: A scalable approach for cyber-physical system simulation",
  IEEE Transactions on 2016 (8 citations).

* AV Papadopoulos, A Ali-Eldin, KE Årzén et al.
  "PEAS: A performance evaluation framework for auto-scaling strategies in cloud applications",
  ACM Transactions on 2016 (8 citations).

* EB Lakew, C Klein et al.
  "Performance-based service differentiation in clouds",
  Cluster, Cloud and 2015 (7 citations).

* S Yeo, MM Hossain, JC Huang, HHS Lee.
  "ATAC: Ambient temperature-aware capping for power efficient datacenters",
  ACM SoCC 2014 (7 citations).
  - Uses trace in evaluation

* D Cheng, P Lama, C Jiang et al.
  "Towards energy efficiency in heterogeneous hadoop clusters by adaptive task assignment",
  IEEE ICDCS 2015 (7 citations).

* S Wu, H Chen, S Di, B Zhou, Z Xie et al.
  "Synchronization-aware scheduling for virtual clusters in cloud",
  IEEE Transactions on 2015 (7 citations).

* A S)rbu, O Babaoglu.
  "Towards data-driven autonomics in data centers",
  Cloud and Autonomic Computing 2015 (7 citations).

* J Li, J Naughton, RV Nehme.
  "Resource bricolage for parallel database systems",
  ACM VLDB 2014 (7 citations).

* A Rosà, LY Chen, R Birke, W Binder.
  "Demystifying Casualties of Evictions in Big Data Priority Scheduling",
  ACM SIGMETRICS Performance 2015 (6 citations).

* J Li, C Pu, Y Chen, V Talwar, D Milojicic.
  "Improving preemptive scheduling with application-transparent checkpointing in shared clusters",
  Middleware 2015 (6 citations).
  - Evaluates efficiency of preemption algorithm

* M Sedaghat, F Hernández-Rodriguez et al.
  "Divide the task, multiply the outcome: Cooperative vm consolidation",
  IEEE 2014 (6 citations).

* L Yazdanov, M Gorbunov et al.
  "EHadoop: network I/O aware scheduler for elastic MapReduce cluster",
  IEEE CLOUD 2015 (6 citations).

* F Kong, X Liu.
  "Greenplanning: Optimal energy source selection and capacity planning for green datacenters",
  IEEE ICCPS 2016 (5 citations).

* K Rybina, W Dargie, R Schöne et al.
  "Mutual influence of application-and platform-level adaptations on energy-efficient computing",
  Parallel, Distributed and 2015 (5 citations).

* B Yang, Z Li, S Chen, T Wang et al.
  "Stackelberg game approach for energy-aware resource allocation in data centers",
  IEEE Transactions on 2016 (3 citations).

* J Liu, H Shen, HS Narman.
  "CCRP: Customized cooperative resource provisioning for high resource utilization in clouds",
  IEEE BigData 2016 (3 citations).
  - The resource usage of short jobs usually does not have certain patterns

* D Lo, L Cheng, R Govindaraju et al.
  "Improving resource efficiency at scale with Heracles",
  ACM Transactions on 2016 (8 citations).

* K Lee, G Buss, D Veit.
  "A heuristic approach for the allocation of resources in large‐scale computing infrastructures",
  Concurrency and Computation: Practice 2016 (2 citations).

* F Xu, F Liu, H Jin.
  "Heterogeneity and interference-aware virtual machine provisioning for predictable performance in the cloud",
  IEEE ToC 2016 (10 citations).

* M Rasheduzzaman, MA Islam, T Islam et al.
  "Task shape classification and workload characterization of google cluster trace",
  IEEE IACC 2014 (4 citations).

* T Saber, A Ventresque, I Brandic et al.
  "Towards a multi-objective vm reassignment for large decentralised data centres",
  Utility and Cloud 2015 (4 citations).

* R Han, J Wang, S Huang, C Shao et al.
  "Interference-Aware Component Scheduling for Reducing Tail Latency in Cloud Interactive Services",
  IEEE ICDCS 2015 (3 citations).

* B Ghit, D Epema
  "Tyrex: Size-based resource allocation in mapreduce frameworks",
  IEEE CCGrid 2016 (3 citations).

* A Havet, V Schiavoni, P Felber et al.
  "GenPack: A generational scheduler for cloud data centers",
  IEEE IC2E 2017 (3 citations).

* K Kc, CJ Hsu, VW Freeh.
  "Evaluation of MapReduce in a large cluster",
  IEEE CLOUD 2015 (3 citations).

* B Luo, S Wang, W Shi, Y He.
  "eCope: Workload-aware elastic customization for power efficiency of high-end servers",
  IEEE Transactions on Cloud 2016 (2 citations).

* A Tumanov, J Cipar, GR Ganger et al.
  "alsched: Algebraic scheduling of mixed workloads in heterogeneous clouds",
  SoCC 2012 (28 citations).
  - "Evidence from available trace analyses suggests that placement constraints
    can range from non-existent to unachievable, even in the empty cluster"

* M Carvalho, F Brasileiro, R Lopes, G Farias et al.
  "Multi-dimensional admission control and capacity planning for IaaS clouds with multiple service classes",
  CCGrid 2017 (NEW).
  - Widely varying aggregated demand over time
  - Uses trace-driven simulation based on Google trace
  - Uses arbitrary, varying SLOs for each job class (production, batch, free)

* X Zhang, Z Huang, C Wu, Z Li, F Lau.
  "Online auctions in IaaS clouds: Welfare and profit maximization with server costs",
  SIGMETRICS 2015 (19 citations).
  - Replays trace for evaluation

* W Wang, D Niu, B Liang, B Li.
  "Dynamic cloud instance acquisition via IaaS cloud brokerage",
  IEEE Transactions on Parallel and Distributed Systems 2015 (11 citations).
  - Uses trace in evaluation

