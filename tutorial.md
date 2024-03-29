# GSP037 -- Tutorial: Detect Labels, Faces, and Landmarks in Images with the Cloud Vision API #

You can list the active account name with this command:

```bash
gcloud auth list
```
Output:

Credentialed accounts:
 - <myaccount>@<mydomain>.com (active)
Example output:

Credentialed accounts:
 - google1623327_student@qwiklabs.net
You can list the project ID with this command:
```bash
gcloud config list project
```
Output:

[core]
project = <project_ID>
Example output:

[core]
project = qwiklabs-gcp-44776a13dea667a6
Full documentation of gcloud is available on Google Cloud gcloud Overview.
Create an API Key
Since you'll be using curl to send a request to the Vision API, you'll need to generate an API key to pass in your request URL.

To create an API key, navigate to APIs & services > Credentials in your Cloud console:

![alt text](https://cdn.qwiklabs.com/8cDO0kBoc2byO7otbM1QFWuYWv0ZZAUH1iBDR200dKA%3D "api_nav.png")

Click on the Create credentials button.
![alt text](https://cdn.qwiklabs.com/OPRnwJAzyxcK1bvJ1Iv7f3FyL5WaNzAqwiu5Yr6oPTI%3D "create_cred.png")

In the drop-down menu, select API key.
![alt text](https://cdn.qwiklabs.com/OyE7y6jsqa%2BB6obwA%2BVYClGCPXy6ER1BKE0f3cLlx7s%3D "api_key.png")

Next, copy the key you just generated and save it to an environment variable to avoid having to insert the value of your API key in each request. Run the following, replacing <your_api_key> with the key you just copied:

```bash
export API_KEY=<YOUR_API_KEY>
```

Upload an Image to a Cloud Storage bucket
Creating a Cloud Storage bucket
There are two ways to send an image to the Vision API for image detection: by sending the API a base64 encoded image string, or passing it the URL of a file stored in Google Cloud Storage. We'll be using a Cloud Storage URL. The first step is to create a Google Cloud Storage bucket to store our images.

Navigate to Navigation menu > Storage in the Cloud console for your project, then click Create bucket.
![alt text](https://cdn.qwiklabs.com/YjeT9%2FI0vbVTm8jEKjpWvL2yQdCuNCqQTnfTf%2Fsabmg%3D "storage_nav")

Give your bucket a unique name and click Create.
![alt text](https://cdn.qwiklabs.com/z%2FofyjLlIzROfsVasYRvkyP0ai5aN4I3HlY1LnmCViw%3D "create_bucket.png")

Upload an image to your bucket
Right click on the following image of donuts, then click Save image as and save it to your computer as donuts.png.
![alt text](https://cdn.qwiklabs.com/V4PmEUI7yXdKpytLNRqwV%2ByGHqym%2BfhdktVi8nj4pPs%3D "donuts.png")

Go to the bucket you just created and click Upload files. Then select donuts.png.
![alt text](https://cdn.qwiklabs.com/Yh4DAGGG5sWsKdgvuVv8mhLb%2Fsr5hMb0OFOji7Wskm8%3D "api_key.png")Bucket_mybucket_upload_files.png

You should see the file in your bucket.

Next, click on the 3 dots for your image and select Edit Permissions.
![alt text](https://cdn.qwiklabs.com/kwtkEJ18%2FGOgDZs915lfQ4ZQvfnPbw8JkECN6egxmKg%3D  "Upload")

Click Add item then enter the following:
Entity: Group

Name: allUsers

Access: Reader

![alt text](https://cdn.qwiklabs.com/RfdadHSPrLA1%2BRcKtdU9HyIhPDGCM4GabDdGVfmq5BI%3D "Add item")

Then click Save.
Now that you have the file in your bucket, you're ready to create a Vision API request, passing it the URL of this donuts picture.

Create your Vision API request
Now you'll create a request.json file in the Cloud Shell environment.

Using the Cloud Shell code editor (by clicking the pencil icon in the Cloud Shell ribbon),
![alt text](https://cdn.qwiklabs.com/O1pSCpaSe6p5nxOMmjQg3Vsf0YwHaqW2bT56hM6Iym0%3D "ThisImage")Cloud Shell ribbon

or your preferred command line editor (nano, vim, or emacs), create a request.json file.

Type or paste the following code into the file:
**Note:** Replace `my-bucket-name` with the name of your storage bucket.
{
  "requests": [
      {
        "image": {
          "source": {
              "gcsImageUri": "gs://my-bucket-name/donuts.png"
          }
        },
        "features": [
          {
            "type": "LABEL_DETECTION",
            "maxResults": 10
          }
        ]
      }
  ]
}

Save the file.

Label Detection
The first Cloud Vision API feature you'll try out is label detection. This method will return a list of labels (words) of what's in your image.

Call the Vision API with curl:
```bash
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY}
```
Your response should look something like the following:
```console
{
  "responses": [
    {
      "labelAnnotations": [
        {
          "mid": "/m/01dk8s",
          "description": "Powdered sugar",
          "score": 0.9861496,
          "topicality": 0.9861496
        },
        {
          "mid": "/m/01wydv",
          "description": "Beignet",
          "score": 0.9565117,
          "topicality": 0.9565117
        },
        {
          "mid": "/m/02wbm",
          "description": "Food",
          "score": 0.9424965,
          "topicality": 0.9424965
        },
        {
          "mid": "/m/0hnyx",
          "description": "Pastry",
          "score": 0.8173416,
          "topicality": 0.8173416
        },
        {
          "mid": "/m/02q08p0",
          "description": "Dish",
          "score": 0.8076026,
          "topicality": 0.8076026
        },
        {
          "mid": "/m/01ykh",
          "description": "Cuisine",
          "score": 0.79036003,
          "topicality": 0.79036003
        },
        {
          "mid": "/m/03nsjgy",
          "description": "Kourabiedes",
          "score": 0.77726763,
          "topicality": 0.77726763
        },
        {
          "mid": "/m/06gd3r",
          "description": "Angel wings",
          "score": 0.73792106,
          "topicality": 0.73792106
        },
        {
          "mid": "/m/06x4c",
          "description": "Sugar",
          "score": 0.71921736,
          "topicality": 0.71921736
        },
        {
          "mid": "/m/01zl9v",
          "description": "Zeppole",
          "score": 0.7111677,
          "topicality": 0.7111677
        }
      ]
    }
  ]
}
```
The API was able to identify the specific type of donuts these are, powdered sugar. Cool! For each label the Vision API found, it returns a:

description with the name of the item.

score, a number from 0 - 1 indicating how confident it is that the description matches what's in the image.

mid value that maps to the item's mid in Google's Knowledge Graph. You can use the mid when calling the Knowledge Graph API to get more information on the item.

## Web Detection
In addition to getting labels on what's in your image, the Vision API can also search the Internet for additional details on your image. Through the API's webDetection method, you get a lot of interesting data back:

A list of entities found in your image, based on content from pages with similar images
URLs of exact and partial matching images found across the web, along with the URLs of those pages
URLs of similar images, like doing a reverse image search
To try out web detection, use the same image of beignets and change one line in the request.json file (you can also venture out into the unknown and use an entirely different image).

Under the features list, change type from LABEL_DETECTION to WEB_DETECTION. The request.json should now look like this:
```console
{
  "requests": [
      {
        "image": {
          "source": {
              "gcsImageUri": "gs://my-bucket-name/donuts.png"
          }
        },
        "features": [
          {
            "type": "WEB_DETECTION",
            "maxResults": 10
          }
        ]
      }
  ]
}
```
Save the file.

To send it to the Vision API, use the same curl command as before (just press the up arrow in Cloud Shell):
```bash
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY}
```
Let's dive into the response, starting with webEntities. Here are some of the entities this image returned:
```console
{
  "responses": [
    {
      "webDetection": {
        "webEntities": [
          {
            "entityId": "/m/0z5n",
            "score": 0.8868,
            "description": "Application programming interface"
          },
          {
            "entityId": "/m/07kg1sq",
            "score": 0.3139,
            "description": "Encapsulation"
          },
          {
            "entityId": "/m/0105pbj4",
            "score": 0.2713,
            "description": "Google Cloud Platform"
          },
          {
            "entityId": "/m/01hyh_",
            "score": 0.2594,
            "description": "Machine learning"
          },
          ...
        ]
```        
This image has been used in many presentations on Cloud ML APIs, which is why the API found the entities "Machine learning" and "Google Cloud Platform".

If you inpsect the URLs under fullMatchingImages, partialMatchingImages, and pagesWithMatchingImages, you'll notice that many of the URLs point to this lab site (super meta!).

Let's say you wanted to find other images of beignets, but not the exact same images. That's where the visuallySimilarImages part of the API response comes in handy. Here are a few of the visually similar images it found:
```console

        "visuallySimilarImages": [
          {
            "url": "https://media.istockphoto.com/photos/cafe-du-monde-picture-id1063530570?k=6&m=1063530570&s=612x612&w=0&h=b74EYAjlfxMw8G-G_6BW-6ltP9Y2UFQ3TjZopN-pigI="
          },
          {
            "url": "https://s3-media2.fl.yelpcdn.com/bphoto/oid0KchdCqlSqZzpznCEoA/o.jpg"
          },
          {
            "url": "https://s3-media1.fl.yelpcdn.com/bphoto/mgAhrlLFvXe0IkT5UMOUlw/348s.jpg"
          },

          ...
]
```
You can navigate to those URLs to see the similar images:

![alt text](https://cdn.qwiklabs.com/vgWkDHchgJbOs4QsNHnrTHKs5%2Fo1rkQWFxMCLtHmrPo%3D "ThisImage")result1.png 
![alt text](https://cdn.qwiklabs.com/w%2BfLMzkJxWN1WummHFhBLmkw5eFFSMFfQUiN6bwAwFA%3D "ThisImage")result2.png 
![alt text](https://cdn.qwiklabs.com/S%2FkNFToGu3UCBBin8FTTTupTpksj6PBcKpa1ZyPqwNk%3D "ThisImage")result3.png

And now you probably really want a powdered sugar beignet(sorry)! This is similar to searching by an image on [Google Images]: https://images.google.com/.

With Cloud Vision you can access this functionality with an easy to use REST API and integrate it into your applications.

## Face and Landmark Detection
Next explore the face and landmark detection methods of the Vision API.

The face detection method returns data on faces found in an image, including the emotions of the faces and their location in the image.

Landmark detection can identify common (and obscure) landmarks. It returns the name of the landmark, its latitude and longitude coordinates, and the location of where the landmark was identified in an image.

Upload a new image
To use these two methods, you'll upload a new image with faces and landmarks to the Cloud Storage bucket.

Right click on the following image, then click Save image as and save it to your computer as selfie.png.
![selfie.png](https://cdn.qwiklabs.com/5%2FxwpTRxehGuIRhCz3exglbWOzueKIPikyYj0Rx82L0%3D "ThisImage")

Now upload it to your Cloud Storage bucket the same way you did before, and make it public.

Updating request file
Next, update your request.json file with the following, which includes the URL of the new image, and uses face and landmark detection instead of label detection. Be sure to replace my-bucket-name with the name of your Cloud Storage bucket:

{
  "requests": [
      {
        "image": {
          "source": {
              "gcsImageUri": "gs://my-bucket-name/selfie.png"
          }
        },
        "features": [
          {
            "type": "FACE_DETECTION"
          },
          {
            "type": "LANDMARK_DETECTION"
          }
        ]
      }
  ]
}

Save the file.

Calling the Vision API and parsing the response
Now you're ready to call the Vision API using the same curl command you used above:
```bash
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY}
```
Take a look at the faceAnnotations object in the response. You'll notice the API returns an object for each face found in the image - in this case, three. Here's a clipped version of the response:

{
      "faceAnnotations": [
        {
          "boundingPoly": {
            "vertices": [
              {
                "x": 669,
                "y": 324
              },
              ...
            ]
          },
          "fdBoundingPoly": {
            ...
          },
          "landmarks": [
            {
              "type": "LEFT_EYE",
              "position": {
                "x": 692.05646,
                "y": 372.95868,
                "z": -0.00025268539
              }
            },
            ...
          ],
          "rollAngle": 0.21619819,
          "panAngle": -23.027969,
          "tiltAngle": -1.5531756,
          "detectionConfidence": 0.72354823,
          "landmarkingConfidence": 0.20047489,
          "joyLikelihood": "POSSIBLE",
          "sorrowLikelihood": "VERY_UNLIKELY",
          "angerLikelihood": "VERY_UNLIKELY",
          "surpriseLikelihood": "VERY_UNLIKELY",
          "underExposedLikelihood": "VERY_UNLIKELY",
          "blurredLikelihood": "VERY_UNLIKELY",
          "headwearLikelihood": "VERY_LIKELY"
        }
        ...
     }
}

boundingPoly gives you the x,y coordinates around the face in the image.
fdBoundingPoly is a smaller box than boundingPoly, focusing on the skin part of the face.
landmarks is an array of objects for each facial feature, some you may not have even known about. This tells us the type of landmark, along with the 3D position of that feature (x,y,z coordinates) where the z coordinate is the depth. The remaining values gives you more details on the face, including the likelihood of joy, sorrow, anger, and surprise.
The response you're reading is for the person standing furthest back in the image - you can see he's making kind of a silly face which explains the joyLikelihood of POSSIBLE.

Next look at the landmarkAnnotations part of the response:

"landmarkAnnotations": [
        {
          "mid": "/m/0c7zy",
          "description": "Petra",
          "score": 0.5403372,
          "boundingPoly": {
            "vertices": [
              {
                "x": 153,
                "y": 64
              },
              ...
            ]
          },
          "locations": [
            {
              "latLng": {
                "latitude": 30.323975,
                "longitude": 35.449361
              }
            }
          ]

Here, the Vision API was able to tell that this picture was taken in Petra - this is pretty impressive given the visual clues in this image are minimal. The values in this response should look similar to the labelAnnotations response above:

the mid of the landmark

it's name (description)

a confidence score

The boundingPoly shows the region in the image where the landmark was identified.

The locations key tells us the latitude longitude coordinates of this landmark.

## Explore other Vision API methods
You've looked at the Vision API's label, face, and landmark detection methods, but there are three others you haven't explored. Dive into the docs to learn about the other three:

Logo detection: identify common logos and their location in an image.

Safe search detection: determine whether or not an image contains explicit content. This is useful for any application with user-generated content. You can filter images based on four factors: adult, medical, violent, and spoof content.

Text detection: run OCR to extract text from images. This method can even identify the language of text present in an image.

## Congratulations!
You've learned how to analyze images with the Vision API. In this example you passed the API the Google Cloud Storage URL of your image. Alternatively, you can pass a base64 encoded string of your image.

Go back to the Qwicklabs page to verify the lab shows as complete.

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

What you've covered
Calling the Vision API with curl by passing it the URL of an image in a Cloud Storage bucket
Using the Vision API's label, face, and landmark detection methods
![ml_quest_icon.png](https://cdn.qwiklabs.com/IApG9d%2FXIEgLgs087RvzuRMv2SCAHo6owBXVZUfyhtQ%3D "ThisImage") ![ML-Image-Processing-badge.png](https://cdn.qwiklabs.com/RGK1oeQbB4Lh3nZadxyeMRveRZuO8WQMNCDyzomMKMM%3D)

Finish your quest
This self-paced lab is part of the Qwiklabs Quests [Machine Learning APIs]:https://google.qwiklabs.com/quests/32 and [Intro to ML: Image Processing]:https://google.qwiklabs.com/quests/85. A Quest is a series of related labs that form a learning path. Completing a Quest earns you a badge to recognize your achievement. You can make your badge (or badges) public and link to them in your online resume or social media account. Enroll in these Quests and get immediate completion credit if you've taken this lab. [See other available Qwiklabs Quests]:http://google.qwiklabs.com/catalog .

Take your next lab
Try out another lab on Machine Learning APIs, like Entity and Sentiment Analysis with the Natural Language API or Awwvision: Cloud Vision API from a Kubernetes Cluster.

Next steps / learn more
Check out the Vision API tutorials in the documentation
Find a Vision API sample in your favorite language on GitHub
Check out the [Entity and Sentiment Analysis with the Natural Language API lab]: https://google.qwiklabs.com/catalog_lab/1113 .
Sign up for the full [Coursera Course on Machine Learning]:https://www.coursera.org/learn/serverless-machine-learning-gcp/
Google Cloud Training & Certification
...helps you make the most of Google Cloud technologies. [Our classes]:https://cloud.google.com/training/courses include technical skills and best practices to help you get up to speed quickly and continue your learning journey. We offer fundamental to advanced level training, with on-demand, live, and virtual options to suit your busy schedule. [Certifications]:https://cloud.google.com/certification/  help you validate and prove your skill and expertise in Google Cloud technologies.

Manual Last Updated Juy 17, 2019
Lab Last Tested Juy 17, 2019
Copyright 2019 Google LLC All rights reserved. Google and the Google logo are trademarks of Google LLC. All other company and product names may be trademarks of the respective companies with which they are associated.