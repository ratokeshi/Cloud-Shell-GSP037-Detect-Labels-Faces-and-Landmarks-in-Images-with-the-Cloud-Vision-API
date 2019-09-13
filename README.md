# GSP037 Detect Labels, Faces, and Landmarks in Images with the Cloud Vision API
A Tutorial on how to complete the GSP037-Detect-Labels-Faces-and-Landmarks-in-Images-with-the-Cloud-Vision-API Lab directly in console. Using the Google Cloud Shell tutorials

[https://google.qwiklabs.com/focuses/6077?parent=catalog](https://google.qwiklabs.com/focuses/6077?parent=catalog)

Overview
The Cloud Vision API lets you understand the content of an image by encapsulating powerful machine learning models in a simple REST API.

In this lab, we will send images to the Vision API and see it detect objects, faces, and landmarks.

What you'll learn
Creating a Vision API request and calling the API with curl

Using the label, face, and landmark detection methods of the vision API


Here are some links: 
[Google Cloud Natural Language API](cloud.google.com/natural-language) and how to use to the Sentiment Analysis feature of the API.

Here are videos for your use:  
[https://www.youtube.com/](From Template replace todo)

To begin the tutorial in a Qwiklab open your Google Cloud Consoles as you normally would.
1.  If you have issues with the console try using incognito mode and go to https://console.cloud.google.com/
2.  If you are in the student session of the GCP Console, click the Cloud Shell icn on the top right that looks like this: ![alt text](https://walkthroughs.googleusercontent.com/tutorial/resources/cloud-shell-icon-v1.svg "Cloud Shell Icon on the top right of the GCP Console")
3.  Enter the following commands to clone this repository and launch the turotial.
git clone https://github.com/ratokeshi/Cloud-Shell-GSP037-Detect-Labels-Faces-and-Landmarks-in-Images-with-the-Cloud-Vision-API.git
cd Cloud-Shell-GSP037-Detect-Labels-Faces-and-Landmarks-in-Images-with-the-Cloud-Vision-API
cloudshell launch-tutorial tutorial.md




```bash
gcloud config configurations create qwiklab-$(date +%s)
gcloud config unset project
gcloud auth login <google5150507_student@qwiklabs.net>
gcloud config set project <from Lab>
cloudshell launch-tutorial tutorial.md
```


To begin the tutorial in your currently authenticated Google account, click on the button given below:
[![Open in Cloud Shell](http://gstatic.com/cloudssh/images/open-btn.png)](https://console.cloud.google.com/cloudshell/open?git_repo=https://github.com/ratokeshi/Cloud-Shell-GSP037-Detect-Labels-Faces-and-Landmarks-in-Images-with-the-Cloud-Vision-API&tutorial=tutorial.md)
