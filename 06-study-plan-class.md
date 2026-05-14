# Lesson 6: Study Plan, Exam Strategy, and What Comes Next

---

Welcome back. Today's lesson is a little different. We are not covering a new AWS domain. Instead, we are going to talk about how to prepare for the actual exam, what to do on test day, and what comes after you pass. Think of this as the coaching session that ties everything together. Pay attention here, because the decisions you make about how to study matter just as much as what you study.

---

## Your Study Timeline

I want to give you two versions of a study plan. One is a four-week plan designed for beginners who are new to AWS entirely. The other is a two-week intensive plan for people with existing IT experience who just need to fill in gaps. Choose the one that fits your situation. Do not try to rush the four-week plan into one week unless you already have strong AWS exposure.

---

### The Four-Week Plan

Week one is your foundation week. Your focus is Domain One, Cloud Concepts, which is twenty-four percent of the exam. In the first two days, cover the fundamentals of cloud computing, the different deployment models, and the six advantages of cloud. Days three and four are for the AWS Well-Architected Framework. Get all six pillars down. Days five and six cover AWS global infrastructure, meaning how Regions, Availability Zones, and Edge Locations relate to each other. Day seven is your review day. Take a short practice quiz and see which topics feel shaky.

During week one, spend some time in the AWS Console if you have not already. Create a free account and explore what the interface looks like. You do not need to build anything yet. Just get comfortable navigating.

Week two is your security week. Security and Compliance is thirty percent of the exam, which makes it the second largest domain. Days one and two are for the Shared Responsibility Model and the basics of Identity and Access Management. Understand users, groups, roles, and policies as distinct concepts. Days three and four go deeper into IAM, covering policy structure and best practices, and add the security services like Shield, WAF, GuardDuty, Inspector, and Macie. Days five and six cover compliance programs and data protection services. Day seven is another review day.

During week two, log into the Console and create an IAM user. Practice creating a group and attaching a policy to it. Enable multi-factor authentication on your account. These hands-on actions will make the exam questions feel more concrete.

Week three is your technology week. Domain Three, Cloud Technology and Services, is thirty-four percent of the exam, making it the largest domain by weight. This is the week you will spend the most time studying. Days one and two cover compute services including EC2, Lambda, ECS, and Elastic Beanstalk. Day three is storage services including S3, EBS, EFS, and the Snow Family. Day four is database services including RDS, DynamoDB, Aurora, and Redshift. Day five is networking including VPC, CloudFront, Route 53, and the load balancers. Day six is additional services including CloudWatch, CloudTrail, SNS, SQS, and CloudFormation. Day seven is your first full practice exam. Take it under timed conditions, sixty-five questions in ninety minutes. Review every question you got wrong.

During week three, launch an EC2 instance, create an S3 bucket, and explore the VPC section of the Console. These are the three most common services on the exam and the most important to have a feel for.

Week four is billing and review week. Domain Four, Billing, Pricing, and Support, is only twelve percent of the exam but it has very specific, testable facts, especially around the support plans. Days one and two cover pricing models, cost optimization, the Free Tier, and the cost management tools. Day three covers the four support plans, Trusted Advisor, and AWS Organizations. Days four and five are for taking two or three full practice exams. Day six is for reviewing your weak areas based on the practice exam results. Day seven is your rest day. Light review only. No new material. Rest is part of preparation.

---

### The Two-Week Intensive Plan

If you have an IT background and are comfortable with networking, servers, and databases, you can compress the plan.

Days one and two: cover Cloud Concepts and Security back to back. The concepts will feel familiar. Focus on the AWS-specific vocabulary and the Shared Responsibility Model. Days three through five: dive into the core services. Compute, storage, database, and networking. Focus on use cases and when to choose one service over another, not on memorizing every feature. Days six and seven: cover the remaining services and take your first practice exam.

In the second week, days eight and nine cover billing, pricing, and support. Memorize the support plan response times. They appear on the exam often. Days ten through twelve are practice exams and review. Take three or four full exams and study the explanations for every question you miss. Days thirteen and fourteen are final review and rest. No new material on day fourteen. You need a clear head on exam day.

---

## Registering for the Exam

Let me walk you through how to actually sign up for the exam, because a surprising number of people put this off and then scramble at the end.

Start by creating an account at the AWS Training and Certification website. If you already have an AWS account for your console access, you still need to create a separate certification account. Once your account is set up, go to your certifications section and choose to schedule a new exam. Select the AWS Certified Cloud Practitioner, which carries the code CLF-C02.

You will then choose your delivery method. The first option is taking the exam at a Pearson VUE testing center in person. You find a location near you, pick a date and time, and show up. The second option is an online proctored exam where a live human proctor supervises you remotely through your webcam while you take the exam at home or in a private office.

The exam costs one hundred dollars. Payment is by credit or debit card. If you need to reschedule, you can do so for free as long as you reschedule more than twenty-four hours before your scheduled time.

One thing worth mentioning is that AWS sometimes makes exam vouchers available through training programs and partner events. If you are going through this guide as part of a formal course, check whether you have access to a discount before paying full price.

---

## Test-Taking Strategies

Let me give you strategies that will actually matter on exam day, not just general advice.

The first strategy is to read every question carefully and look for the keyword that changes the meaning. The exam uses specific words intentionally. When a question asks for the most cost-effective solution, it is asking you to compare pricing models. When it asks for the best approach, it usually wants the AWS-recommended managed service rather than a custom-built solution. When it uses the word least in terms of effort, it is pointing toward a fully managed service that requires minimal setup. When a question uses the word not or except, it is asking you to find the wrong answer, not the right one. That is a reversal that catches many people off guard.

The second strategy is to eliminate wrong answers first. On a four-option question, you can often immediately rule out two answers that are clearly off-topic or that belong to a different domain. Once you are down to two candidates, the decision becomes much easier.

The third strategy is to flag and move on. If a question is genuinely hard, do not stare at it for three minutes. Read it carefully, make your best guess, flag it, and continue. You will answer the remaining questions faster, and when you come back to the flagged ones, the context shift often helps you see the question differently.

The fourth strategy is to never leave a question blank. The exam has no penalty for wrong answers. A blank answer counts as wrong automatically. A guess gives you a chance. Never submit your exam with unanswered questions.

The fifth strategy for multiple response questions is to pay close attention to the instruction. Some questions say select two and some say select three. You need to hit the exact number. Select too few or too many and the entire question counts as wrong.

---

## Time Management During the Exam

You have ninety minutes and sixty-five questions. That works out to roughly one minute and twenty-three seconds per question on average. In practice, some questions will take you fifteen seconds and some will take two minutes.

Here is the approach that works well for most people. In your first pass, work through all sixty-five questions with this mindset: answer questions you know immediately, take up to ninety seconds on questions that require thought, and flag anything that feels genuinely uncertain without spending more than two minutes on it. Aim to finish your first pass with about twenty minutes remaining.

In your second pass, return to your flagged questions. By now your brain has had a break from those specific questions, and many of them will feel clearer. Work through them and lock in your answers.

Use your final five to ten minutes to do a quick scan. Verify that no question is blank. Double-check any select two or select three questions to make sure you chose the exact right number. Then submit with confidence.

---

## How to Approach Each Question Type

There are five main question patterns on this exam and each one has a reliable approach.

Scenario-based questions describe a situation and ask which service or approach fits best. Start by identifying the key requirements in the scenario. If it says the data is accessed infrequently, that rules out Standard storage. If it says millisecond response times, that points toward a cache like ElastiCache or a fast database like DynamoDB. Match the requirement to the service characteristic, not just the service name.

Cost optimization questions ask which option is the most cost-effective. Apply the pricing model logic you learned in Domain Four. If the workload runs continuously and the usage is predictable, Reserved Instances or Savings Plans beat On-Demand. If the workload can tolerate interruptions, Spot Instances provide the deepest discount. If the workload is irregular or temporary, On-Demand is the right answer.

Security questions ask which option is the most secure or which approach follows best practices. Always lean toward the principle of least privilege. Prefer IAM roles over access keys. Prefer encryption at rest and in transit. Prefer managed security services over manual solutions. Never choose an option that requires storing credentials in your application code.

High availability questions ask which design ensures the application stays up during failures. Look for multi-Availability Zone deployments, load balancers, and Auto Scaling. A design that keeps everything in a single Availability Zone is almost never the right answer for high availability.

Except and not questions are the ones that trip people up most often. You are looking for the answer that does not belong, not the one that does. Read the question stem twice before looking at the options. Treat it as a true-or-false evaluation for each option.

---

## Common Pitfalls to Avoid

Let me walk you through the mistakes students make most often on this exam so you can avoid them.

The first pitfall is confusing similar service names. EC2, ECS, EKS, and EBS are four completely different things despite looking alike. EC2 is your virtual server. ECS is container orchestration for running Docker containers. EKS is the managed Kubernetes service. EBS is block storage that attaches to an EC2 instance. Know these four without hesitation.

A related confusion is between S3, EBS, and EFS. S3 is object storage that is accessible from anywhere over the internet and can store virtually unlimited data. EBS is block storage that behaves like a hard drive and can only be attached to one EC2 instance at a time. EFS is a network file system that can be mounted by multiple EC2 instances simultaneously. The key distinguishing question is: how many servers need to access this storage at once?

The second pitfall is misremembering the Shared Responsibility Model. AWS is responsible for security of the cloud, meaning the physical data centers, the network infrastructure, and the underlying hardware and software that runs the services. The customer is responsible for security in the cloud, meaning the data you store, the applications you deploy, the IAM permissions you grant, and the encryption you configure. When the exam describes a breach or a vulnerability and asks who is responsible for preventing it, think carefully about which layer it sits in.

The third pitfall is mixing up support plan response times. Basic has no technical support at all. Developer provides email support during business hours only, with a twelve-hour response for system impaired situations and twenty-four hours for general guidance. Business provides twenty-four seven phone, email, and chat, with a one-hour response for business-critical issues. Enterprise provides everything Business does plus a Technical Account Manager and a fifteen-minute response for mission-critical system failures. These numbers are tested directly. Memorize them.

The fourth pitfall is overthinking scenario questions. The exam tends to favor simple, managed service solutions over complex custom-built ones. When a question offers you the choice between setting up your own server to handle authentication versus using Amazon Cognito, the exam almost always wants the managed service. When a question offers you the choice between writing your own code to process messages versus using SQS, the exam almost always wants SQS. Trust the pattern: AWS recommends AWS managed services.

The fifth pitfall is forgetting to look for the not or except keyword. These questions look exactly like regular questions but require the opposite logic. Train yourself to read the last line of the question before you look at the answer choices, because that is where the keyword usually lives.

---

## Resources Worth Using

For official AWS resources, the AWS Cloud Practitioner Essentials course on AWS Skill Builder is free and covers all four domains in about six hours. It is a good overview even if you are not a beginner. The official AWS Exam Guide is a downloadable document that lists every topic covered on the exam. Use it as a checklist to verify you have covered everything. The AWS Well-Architected Framework whitepaper is worth reading at least once, particularly the sections describing each of the six pillars.

For practice questions, the official AWS practice exam costs twenty dollars and contains twenty questions in the same format and difficulty as the real exam. It is one of the best investments you can make in your preparation. Tutorials Dojo is a widely recommended third-party practice test provider with detailed explanations for each answer. Many students find their practice exams slightly harder than the real thing, which is a good way to build confidence.

The AWS Free Tier account is your most underrated resource. Many concepts on this exam feel abstract until you actually click through the Console and see how a service is configured. Spending thirty minutes launching an EC2 instance teaches you more about the difference between instance types, AMIs, and security groups than a full hour of reading.

---

## The Final Checklist

In the week before your exam, make sure you have taken at least three full practice exams under timed conditions and that you are scoring consistently above eighty percent. Review the Shared Responsibility Model one more time. Memorize the support plan response times. Review the S3 storage classes and when to use each one. Review the EC2 pricing models and their use cases. Make sure you can distinguish between CloudWatch, CloudTrail, and AWS Config without hesitation.

The day before your exam, do a light review only. Do not try to learn new material the night before. It will not help and it will increase anxiety. Lay out your identification documents. If you are taking the online proctored exam, run the system test to verify your equipment, clear your desk, and confirm your room is quiet and private. Then get seven or eight hours of sleep. I am serious about this. Sleep is a more effective exam preparation tool than cramming at midnight.

On the morning of your exam, eat a good breakfast. Arrive at the testing center fifteen to thirty minutes early or log in early if you are taking it online. Take a few slow breaths before you start. You have put in the work. Trust your preparation.

During the exam, read each question carefully. Watch for the keywords. Flag anything you are unsure about and keep moving. Manage your time. Answer every question before you submit. Review your flagged questions. Then submit.

---

## After the Exam

When you finish and submit, the testing system will show you a preliminary pass or fail result immediately on the screen. Your official score report, which includes a numerical score on the one hundred to one thousand scale and a breakdown of your performance by domain, arrives in your AWS Certification account within five business days.

If you pass, AWS will issue a digital badge through the Credly platform within one to two weeks. You can share that badge on LinkedIn, add it to your resume, and use the link to let others verify your certification. Your certification is valid for three years from your exam date. AWS will email you reminders before it expires, and you can recertify by retaking the same exam or by passing any AWS associate-level certification, which automatically renews your Cloud Practitioner.

If you do not pass, do not be discouraged. Many people who go on to successful cloud careers did not pass this exam on the first attempt. Review your score report carefully. The domain breakdown will show you exactly where you lost points. Focus your review on those areas, take more practice exams, and schedule a retake when you feel ready. AWS requires a fourteen-day waiting period between attempts, and there is no limit on how many times you can try.

---

## Key Numbers to Lock In Before Exam Day

Let me give you the most testable numbers from across all four domains, all in one place.

S3 durability is eleven nines, which is ninety-nine point nine nine nine nine nine nine nine nine nine percent. Every Region has a minimum of three Availability Zones. There are over four hundred Edge Locations globally. Lambda functions can run for a maximum of fifteen minutes. A single RDS instance can have up to five Read Replicas. The EC2 Free Tier gives you seven hundred and fifty hours per month for the first twelve months. Lambda's one million requests per month is always free, no expiration.

For support response times: Developer is twelve hours fastest. Business is one hour fastest for business-critical issues. Enterprise is fifteen minutes fastest for mission-critical issues. The minimum cost for Basic is zero. Developer is twenty-nine dollars. Business is one hundred dollars. Enterprise is fifteen thousand dollars.

For the exam itself: ninety minutes, sixty-five questions, seven hundred is the passing score on a scale of one hundred to one thousand, and there is no penalty for wrong answers.

---

## What Comes After This Certification

Once you have your AWS Cloud Practitioner certification, you have several directions you can go. Many people use this credential as the first step toward the associate-level certifications. The Solutions Architect Associate is the most popular next step and builds directly on the foundation you established here. The Developer Associate and the SysOps Administrator Associate are also strong options depending on your career direction.

Beyond the associate level, AWS offers specialty certifications in areas like security, machine learning, advanced networking, and data analytics. Each specialty requires deeper expertise in a specific domain, but your Cloud Practitioner and associate credentials give you the vocabulary and foundational understanding that makes those advanced certifications accessible.

The most important thing you can do after passing this exam is not to stop. Use your AWS account. Build real projects. Read about new services when AWS announces them. Cloud computing moves fast, and staying current is how you turn a certification into genuine expertise.

---

You now have everything you need: the knowledge from the four domain lessons, the strategy to apply it on exam day, and the plan to get from here to the test center or your home setup. Revisit this lesson the week before your exam as a final tune-up. Good luck. You have put in the work, and you are ready for this.

---
