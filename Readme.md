# Adidas Coding Challenge #
This is the repository for the pipeline, I've developed for the adidas coding challenge. 

## About me ##

I'm Pascal Schoen and I'm a software developer at Digital Creation at adidas. I've been working there since almost 5 years now - 2 years as a working
student and 3 years as a full-time employee. Here I've mostly worked on 3D creation topics around Footwear, for example an integration of the "Principled" shader, 
which was developed by Disney, into the open-source 3D software Blender as well as more topics in 3D geometry creation.

## Manual Steps to execute ## 
__Create jenkins namespace__

    kubectl create -f .\minikube\jenkins-namespace.yaml

__Create a persistent volume for Jenkins to keep the config when shutting down the cube__

    kubectl create -f minikube/jenkins-volume.yaml

__Install Jenkins in Kubernetes__

    helm install --name jenkins -f helm/jenkins-values.yaml stable/jenkins --namespace adidas-challenge

__Futher steps__

Open Jenkins in webbrowser and configure a new pipeline project.

This pipeline project will get the pipeline script from SCM.

The SCM is Git and the Repository URL is the url to this repository (https).

## TODOs ##

There are a few missing parts, which are the quality gates, some parts of the infrastructure setup (described above) as well as the autoscaling capabilities. This was not finished due to limited time in the last month and issues with the correct setup of Jenkins and Kubernetes on my local mashine. There still seems to be an issue with the settings, which causes errors to pop-up during the build process, which I couldn't manage to fix yet.

## Last comments ##

This was actually a very fun challenge for me as I have not worked with Kubernetes and Jenkins yet. I only did some basic tasks with Docker. My main background is, as said sooner, in computer graphics, where I'm developing mostly in C++, C# and Python. There, I didn't have too many touch points with such a process. I learned quite a lot during the past week and will, nonetheless of the outcome of my application, try to finish this challenge. Furthermore, I want to use my learnings in my daily work. One project, where I can apply this is Digital Creation End2End and more specific the application "Creation Portal".