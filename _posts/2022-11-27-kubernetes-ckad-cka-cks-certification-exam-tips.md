---
title: 'Kubernetes CKAD CKA CKS Certification Exam Tips'
date: '2022-11-27'
author: marcolenzo
layout: post
permalink: /kubernetes-ckad-cka-cks-certification-exam-tips/
categories:
    - 'DevOps'
tags:
    - Kubernetes
    - Kubernetes Certification
    - CKA
    - CKAD
    - CKS
---

Kubernetes is hands down the most disruptive technology that we have seen in the IT industry in recent years. It is loved by software developers, operators, and business owners. It is pure expression of what DevOps means from a technical perspective. Holding a Kubernetes certification is something I strongly advice to any serious IT practitioner.  
  
Obtaining the certification is tough but rewarding. **In this post, I guide you through the process of choosing the right certification for your role and goals. I give you tips to prepare for the exams and clear them with confidence.** I also share my first hand experience to make sure you have no surprises along the way, especially during the exam session.

## CKAD vs CKA vs CKS curriculum

CKAD, CKA and CKS are part of the official Cloud Native Computing Foundation (CNCF) certifications and their full curriculum is available in the [CNCF Curriculum repository](https://github.com/cncf/curriculum).

![CKAD vs CKA vs CKS](/assets/img/2022/11/cka-ckad-cks.jpg)

The **Certified Kubernetes Application Developer (CKAD)** exam certifies that you can design, build, configure, and expose cloud native applications for Kubernetes. This certification is mostly fit for software developers or DevOps engineers who interact with Kubernetes only to **deploy and monitor their applications**.

The **Certified Kubernetes Administrator (CKA)** program certifies that you have the skills, knowledge, and competency to perform the responsibilities of Kubernetes administrators. This certification is what you need if you need to **create and manage Kubernetes clusters**. Typically this is the role of system administrators, DevOps / DevSecOps / Platform engineers.

The **Certified Kubernetes Security Specialist (CKS)** program provides assurance that the holder is comfortable and competent with a broad range of best practices. CKS certification covers skills for securing container-based applications and Kubernetes platforms during build, deployment and runtime. This is the right certificate if you want to learn the principles and practical solutions necessary to **secure your environments and pipelines**. **This is a hands on certification** that has the CKA certification as a pre-requisite. It is **not suitable for Information Security Specialists who act mostly as auditors or consultants**.

## Preparing for the exam

Whatever certification you choose, the exam is a practical test where you need to solve circa 20 problems in two hours. For this reason, **it is important that you practice extensively** before sitting for it. Do not be tricked by the fact that you can access the documentation during the exam. You will not have time to think too much about a problem.

The best way to prepare for the exam is to follow a course that offers you both video lectures and interactive environments where you can put into practice what you learn along the way.

- CKAD <https://www.udemy.com/course/certified-kubernetes-application-developer/>
- CKA <https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/>
- CKS <https://www.udemy.com/course/certified-kubernetes-security-specialist/>

![CKS Course](/assets/img/2022/11/cks-course.jpg)

Except for the CKAD, I tried all courses personally and I guarantee they are extremely good. The authors make a good job at explaining all topics part of the curriculum. The labs are a godsend! You will have chance to practice on real Kubernetes environments while troubleshooting specific problems which will be similar to what you encounter in the exam.

![CKS Lab](/assets/img/2022/11/killercoda.jpg)

## The simulator

When you acquire the exam with the Linux Foundation, you are given **two free exam simulator sessions** on [Killer Shell](https://killer.sh/). Do not waste them! Make sure you prepare before you attempt the simulations. **You need to approach them as if they were a real exam** to get the most benefit.

Currently the simulator session lasts for 36 hours. You still have a two hours timer on the top left of the screen after which you will be able to access score and solutions. **You need to start the session only when you know you have two free hours to dedicate to the exam simulation without distractions**. You need to replicate as much as possible the conditions of the real exam to get a proper indication of whether you are ready to go for it or not.

![Kubernetes Exam Simulator](/assets/img/2022/11/simulator.jpg)

**If you score poorly in the simulator, there is no chance you will pass the exam**. Despite the simulator claims to be more difficult than the real exam, there are other factors that make the real exam more challenging which I will describe later.

**The tasks in the simulator are always the same**. Thus, attempting **a second simulation might give you a false sense of confidence by scoring high on questions you knew beforehand**. It makes sense to use it only if you let enough time pass for you to forget what was its content.

Make sure to **identify your weak topics** **during the simulation and dedicate time to strengthen your knowledge on them**. After all this is the real goal: learning the concepts and not just achieving the certification. During the exam the questions will be different and at times a slight variation of the problem can be enough to confuse you if you are not really conversant about the topic.

## The proctoring platform

I already anticipated that **the exam session is challenging**. One of the factor making the exam difficult is the proctoring platform. There are many aspects that made the proctoring platform very challenging to me, especially the first time. I will try to give you a detailed description so you avoid unpleasant surprises. This is the link to its introduction video <https://content.psionline.com/online-proctoring>

### **Proctors are very strict**

Your entire exam is being monitored through a live feed of your screen, camera and microphone. **Proctors are strict and will not allow any movement.** You are expected to look exclusively towards the monitor and nothing else. Even your hands have to be constantly on the keyboard. I was warned for having covered my mouth (just to scratch my nose a little bit) or having looked for a split moment away from the monitor (while pondering how to deal with an issue).   
  
**This is done to ensure the exam is fair and no cheating is involved. It is what makes the certification respectable.** However, it can put pressure on top of the one you already have because of the technical challenges posed by the tasks. It is good that you prepare mentally for it.

### **You need to have a good camera**

On my second examination my camera decided to blur my passport whenever I tried to show it to the proctor. I think we dedicated a good 15 minutes to it and I was really concerned the proctor was going to give up and just cancel my session. I have to say he (or she) was extremely understanding and he did try to help me as much as he could. I really thank him.  
  
Despite his kindness, those 15 minutes put so much stress in me that I ended up buying a high end camera the next day, despite I had passed the exam!

### **Tidy Desk and room**

**You desk has to be tidy. No electronic equipment other than keyboard, mouse and monitor (one only) can be present on it.** You can only have a glass or bottle of water. If you have a bottle make sure it has no labels. If you have a printer nearby, you need to show that there’s no paper loaded in it.

![Tidy Desk](/assets/img/2022/11/desk.jpg)

### **Only one monitor allowed (Laptops are bad)**

I did not experience this first hand but I did some research before performing the exam. Apparently, if you use a laptop you will be forced to use that as your only monitor. That will make your exam extremely difficult since the RDP session is already happening on just the right-end side portion of the screen estate, since the left-end side is used to describe the tasks.

Be careful since you might have read other blogs that were written when two monitors were allowed. That has changed in mid 2022 due to a planned migration to the PSI Bridge proctoring platform. You can read the announcement in this post <https://training.linuxfoundation.org/bridge-migration-2021/>.

### **No personal bookmarks allowed**

Before it was customary to prepare bookmarks for your exam since you would be using your browser. With the new platform you cannot leverage them as you will be RDPing to a Linux box and you will be forced the Firefox browser installed within. In the task description you will get links to the relevant documentation. However, my advice is to become very comfortable searching material through the search function on <https://kubernetes.io/docs/home/>

![Kubernetes Documentation](/assets/img/2022/11/k8s-docs.jpg)

### **Make sure your browser is the only process running before the check-in**

During the check-in, you will be asked to install a software that guides you through all the steps, including verification of software that runs on your system. **Almost nothing is allowed!** It is better that you prepare in advance because some programs might run as services and it might take you quite some time to figure out how to get rid of them.

For instance, you cannot run HyperV, Adobe Creative Cloud and Gaming Software.

### **Candidate handbook**

Read well the [candidate handbook](https://docs.linuxfoundation.org/tc-docs/certification/lf-handbook2) in detail and make sure to comply with all the requirements before you start the session.

## The exam

### The time factor

The exam lasts two hours and you will have to deal with anything between 15 and 20 tasks. That means that on average **you have 6 to 8 minutes to dedicate to each task**.

**The tasks have different difficulty and contribute a different percentage to the final mark.** This percentage is clearly denoted in the task description. You can easily browse tasks and switch from one task to another through a dropdown. **You can flag tasks** to highlight you might want to tackle them later. which you want to tackle later. This is a very important feature.

**You do not have to solve the tasks in sequence.** In fact, **my advice is to skip problematic tasks** (those for which you do not have a solution immediately or that give you some surprising error) so you can focus on scoring all those you feel certain about. What will happen is that you will end up solving the vast majority of the tasks well under the 6 to 8 minutes timeframe, giving you much more time to dedicate to the complicated tasks. In my CKS exam, I completed all tasks except two in 90 minutes. I then dedicated the remaining 30 minutes to the tasks that were giving me issues.

### Technical tips

At the beginning of each task, **you are given a string to select a `context `in your `kubeconfig` that connects you to the target environment**.

Make sure to **explore the environment** by listing at least the `nodes `and possibly the `namespaces`.

```shell
marcol@jammy:~$ k config use-context admin@cks-lab 
Switched to context "admin@cks-lab".

marcol@jammy:~$ k get nodes --show-labels 
NAME    STATUS   ROLES           AGE   VERSION   LABELS
controlplane Ready    control-plane   20d   v1.25.3   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=controlplane,kubernetes.io/os=linux,node-role.kubernetes.io/control-plane=,node.kubernetes.io/exclude-from-external-load-balancers=

marcol@jammy:~$ k get ns
NAME                  STATUS   AGE
calico-apiserver      Active   20d
calico-system         Active   20d
default               Active   20d
kube-node-lease       Active   20d
kube-public           Active   20d
kube-system           Active   20d
storage-provisioner   Active   19d
tigera-operator       Active   20d
```

Not all tasks are performed on the terminal node. **Some tasks require you to connect to the actual nodes in the target cluster** to be able to verify and manage their configuration. You can simply connect to them using `ssh <node name>`

**Take backups and comment any file you edit.** Since the correction process is (probably) fully automated, a misconfigured environment can lead you to lose all points you would have accumulated solving issues on it. This is particularly true for all those tasks that involve configuring the `kube-apiserver` or `etcd`. When those two services are misconfigured, it is impossible to retrieve anything from the Kubernetes API, so make sure to never leave them in a corrupted state. Use `systemd` and `crictl` to check theirs status.

**Check your answers whenever cheap to do**. It often takes very little to verify that your tasks have been completed correctly. Sometimes it is just a matter to `cat` a file or perform a very simple `kubectl `command. Don’t be lazy as you might be leaving points behind without noticing.

**Copy paste when possible**. The names of target resources are copy pastable just with a click. Avoid typing as a typo might mean 0 marks for that particular task. Imagine you need to create a `webapppod` but you create a `webappod` instead. You don’t want to take these risks.

## Conclusion

It does take time to prepare for the exam but I can assure you it is very rewarding. Make sure to take time to practice over and over every item defined in the [CNCF Curriculum](https://github.com/cncf/curriculum). Read the [candidate handbook](https://docs.linuxfoundation.org/tc-docs/certification/lf-handbook2) and prepare ahead for the proctoring session.

That’s all for now! Good luck.  
  
Feel free to hit me with questions in the comments section.