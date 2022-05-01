
Final Project for Cloud Computing

# cs4843 Project

## Table of Contents
[Project](#project-overview)

[AWS Plug ins](#aws-plug-ins)

[Testing and Project Report](#testing-and-project-report)

[How it all fits together](#how-it-all-fits-together)

[Code and Look](#code-and-look)

[App UI](#app-ui)

[Final Notes and Summary](#final-notes-and-summary)



### Using AI for good rather than Evil

[Top](#table-of-contents)


![image](https://user-images.githubusercontent.com/93015308/166163936-f04c1265-d119-4257-88cf-5e18b8be11cd.png)


# Project Overview

[Top](#table-of-contents)

Amazon and other services within the advertisement ecosystem, (Google, etc) currently use past data to build future guesses to determine useful information about us.  How many times have you opened up your browser or phone or google search and found something you either did not know you needed, or exactly what you were thinking about but had not yet shown interest?

One hundred years ago this would be considered highly unlikely. Three hundred and someone would be getting burned at a stake.  I believe that this can actually be used to make our lives easier, but differently to how it is currently used. Shopping lists. If customer usage of reoccurring purchases could be easily determined then forecasted then the chore of making lists and keep pantries stocked could become automated. AWS offers demand forecasting, image and video analysis as well as “Personalized Recommendations” to allow machine learning to do this.  There are already parts of this in use, see: https://news.samsung.com/us/new-food-ai-looks-inside-fridge-help-find-perfect-things-cook-already/
This appliance already has cameras and can use video/photo analysis, though it uses it for a much more complicated idea then I am proposing (I’m not an AI expert, or member of a team of them!) Additionally, it is likely beyond my ability and this project’s scope to automate the buying process. Given more time and admittedly more skill this would be the natural end point of my idea. I hate grocery shopping. I am bad at it. I function much more like a hunter than a gatherer, I pick targets and the collateral items along the path also end up in my cart. 


## Major Key Points

#### The app will create a list that can be interacted with by the customer.
#### The app will connect to AWS to use the 3 machine learning points effectively.
#### The app will need to save user data and project by which dates the list needs updated.

The app will need some way of knowing what was already purchased, or verifying its own stock. (It should be able to see what is in the pantry or fridge) This will avoid “holes” in stock if for example the store was out of something, while the list was checked off by the customer.

## AWS Plug ins

[Top](#table-of-contents)

[Demand forecasting](#demand-forecasting-aws-forecast)

[Image and video analysis](#image-and-video-analysis-amazon-rekognition)

[Personalized Recommendations](#personalized-recommendations-aws-personalize)


### Demand forecasting (AWS Forecast)

![image](https://user-images.githubusercontent.com/93015308/166164209-ac5684a1-8596-481c-a7cd-987cb5d2be36.png)

This particular aspect is really the key to everything else. Without it there is no way to make any other part of this more efficient than simply a camera in your pantry and fridge. It takes historical data, and related data (in our case, you probably want a turkey or ham for thanksgiving, but maybe not because its “Tuesday”) combines it, then forecasts the future of your list.

### Image and video analysis (Amazon Rekognition)

![image](https://user-images.githubusercontent.com/93015308/166164303-cb3c78c3-c83e-474c-91d0-810c49c97e74.png)

This aspect will be used in the absolute lowest common denominator sense. The impressive abilities of this AWS aspect will be dumbed down. Greatly. Able to determine if a human is wearing their PPE, in our case we will use it to get maximum information out of stock on hand (and importantly to the above AWS service, what is NOT in stock) An example of this is general video AI I would expect to be good enough to get a choice right between “Is that ketchup, or barbeque sauce” while with ours we need to be very good at telling the brand. I hate fancy BBQ sauce, and would hate to be mailed a condiment I’ll never use.


Below we can see that the AI is able to do an amazing job of isolating very similar items. 

![image](https://user-images.githubusercontent.com/93015308/166164123-0ea43285-8341-4d2d-94ab-77ead1fa8dfc.png)


### Personalized Recommendations (AWS Personalize)

![image](https://user-images.githubusercontent.com/93015308/166164335-3e00e447-3c87-45a9-acb6-371a738a97bc.png)

This final one ties it all together. It will actually do the recommendation of items to be used, and will integrate it into existing systems.

It also encrypts data that it uses, which is always important. Quite a lot can be scrapped from things as benign as a shopping list. From telling where you are, what you like, to if you’ve been out of the house for some time. We must keep this data secure!

# Testing and Project Report

[Top](#table-of-contents)

In theory my overview appeared awesome. All the components are in the same ecosystem and advertised plug and play integration both together and with the data I would use as the base set. I could use data I input as photos, and it accepted an inital set based on CSV files. These are commonly used here at UT and I am quite familar with them. In practice AWS did not deliver. Their basic or "free" version of photo recognition is tuned to photos of people, vehicles, and natual settings. It struggled with anything outside of that set.

### Photo recognition testing

This is a picture of two goats. One is labeled “dog” the other “Bicycle”

![image](https://user-images.githubusercontent.com/93015308/166164803-385b2cf1-0ed0-4d7a-9b22-2bef6365e71c.png)
![image](https://user-images.githubusercontent.com/93015308/166164805-c89cd7e7-8d98-47f8-9e93-726de2801acb.png)

More to our purpose of the project we have milk without label and chocolate syrup label side out.

![image](https://user-images.githubusercontent.com/93015308/166164838-736266d6-7f8e-4b9b-b5af-e06fb7cc095f.png)
![image](https://user-images.githubusercontent.com/93015308/166164843-e4b1ed19-5783-4cdb-b5db-58291a0464b1.png)

It gets closer, but farther away as well. Unable to label anything in the photo, but at least getting into the ballpark on the scene, stating “Appliance” and “Shelf”. Technically every guess is correct, but we were hoping for the high precision that was pitched.
Integration of this appears to be an exercise in futility. Even if we do, it won’t be able to get us actionable data. Given time we should be able to train an AI. But we’re on a deadline here, so for the rest I will assume we did this. Otherwise, all other steps will not work without shortcutting data forecasting manually through just placing data in the CSV dataset. 


### Dataset Forecasting

![image](https://user-images.githubusercontent.com/93015308/166164895-6d507df3-3a64-4054-8484-aac0ab3bfad7.png)

Here we will place all the “attributes”. Note the JSON format, and the items needing filled in. This is perfect for our application. We give it a human readable name as a string, then a timestamp and finally our “demand” which for our application is the rate of use.

### AWS Personalize

![image](https://user-images.githubusercontent.com/93015308/166164937-c458b1b8-f7c9-4f58-ba12-199618943710.png)

This is the other side of the forecast type and time. Tracking the user’s next “demand” by way of events. Thanksgiving will require things not typically recommended and documented. Using this we will populate our dataset with things not typically added (like milk, or eggs we constantly may need those) with this one we can also track things the user may not actually know they want. Perhaps someone that does not buy milk would like to try a non-dairy substitute or the like. We would get this data from trends among other users.

# How it all fits together

[Top](#table-of-contents)

This is the extremely hard part. Not just because I am a one-man team that had a bit of a hard semester about half way through, but because my basic assumption is that the pieces would either fit together (they do) but that the lowest level one would be the most likely to be plug and play. With the failure of photo recognition, I am forced to figure this out manually.
Attempts to train the model caused this:

![image](https://user-images.githubusercontent.com/93015308/166164957-84d2f411-2662-41de-911c-2d41e9016984.png)

Which meant to do what I want I’d possibly end up paying. Rather than do this (As I am currently rather broke) I’ll assume it works

Key to getting actionable data out of image processing is "labels" 

![image](https://user-images.githubusercontent.com/93015308/166165022-5126ade4-824e-43f4-a01b-0ee4399e3283.png)


This is the output from our test photo. It appears that the AI is trained primarily on things outdoors and not alive. After trying many photos, I discovered that the “confidence” actually stays higher than one would imagine when it fails. I believe, based on my own trials, that it first picks agnostic labels and works its way in. “Bottle” for example is not wrong, but is also not helpful. Could it be a bottle of milk as shown? Perhaps a bottle of hot sauce?

Contrasting this, it was able to identify my tractor with an extremely high confidence rating.

![image](https://user-images.githubusercontent.com/93015308/166165159-243b54c7-6880-4c4c-9c51-3e350905ff1f.png)
![image](https://user-images.githubusercontent.com/93015308/166165162-cc08486d-7f2a-462a-9a6a-0c1f23c7d42f.png)

I believe that all these problems can be easily solved. Training the model is automated, all I need is a huge data set of photos and enough money to have AWS crunch that over a few days to weeks.

Below is information from the above picture. You can see how it nests labels and works through what it could be looking at. This is really where the strength of this path on my project would sit. As it picks up photographic clues it runs through paths in a hierarchy. The tractor is a “vehicle” which is a child of “transportation” and it (mistakenly) believes a tractor is the parent of “bulldozer”. I’d love to own a bulldozer! That would make a lot of ranching stuff I do easier. However, tractors are cheaper and require maintenance I can do. All of this is based on training AWS has done for their own model. You are encouraged to do your own training, as it will be more specific and therefore more accurate.

![image](https://user-images.githubusercontent.com/93015308/166165197-bcf609d9-d36e-4e54-9005-88c0cdbc7196.png)

So, with training assumed to be done we can create a data dump through CSV, for your own use, or we can create a “dataset” which allows easy importation into the other pieces of our project. 

![image](https://user-images.githubusercontent.com/93015308/166165308-e62b4ce1-9aea-4f3c-9804-b050d465cf4f.png)

Here we import our photo dataset into Forecast. We would need an extremely high level of accuracy because Forecast will import all the labels we found in training. After that is complete, we still need frequency data. This will have to be trained as well. You might only need milk once a week, or nearly every day.  We do not get that from simple photo recognition, but rather over time. Were I to actually build the app I would run photo recognition often and use the deficit of items drive the “demand” label. I am guessing this would be automated because the dataset in Forecast is specifically tuned (through set up) to track stock in a retail setting. This of course would be by design. 

Finally Personalize would track events which would do two things: seasonal purchases and suggestion of new items based on user trends. Personalize also runs on datasets which makes integration automated.


# Code and Look

[Top](#table-of-contents)

The method used for this is JSON and CSV files. The image recognition can be trained which will give us data based on how sure something is and where it sits. Datasets unique to AWS are also created which is the method perferred. This allows real-time sharing accross my different plugins and the consumption and processing of information. 

## App UI

[Top](#table-of-contents)

![image](https://user-images.githubusercontent.com/93015308/166165322-af8d7764-ef86-453c-85d5-f045b4675b80.png)

Here we have a prototype for the App screen. The real genius is the calendar mode. Using this a customer would be able to pick a date into the future and have the list populate not only what they currently have, but also what they will use and therefore need to that date. This would leverage data already obtained about need, and marry it to demand and stock. Perhaps expiration could be factored in, though that would need to be manually added unless the photo and video could access the stamped best by dates. Definitely something an evolution of this project to actual use would want. 

# Final Notes and Summary

[Top](#table-of-contents)

Ultimately, I was unable to get the image recognition to work. This crippled everything else as it is the most basic building block of this project. The theory is sound, the only thing missing is a dataset to work through on the server side. I'm not an App developer in any sense of the word, but was able to mock up the screen above and add in everything it would need (minus a log in screen) I believe that the end state is highly obtainable, all that it needs is a properly trained dataset that can (with high precision) guess grocery items, and over time be trained to guess intervals of each stock item's lifespan. The app portion is quite simple and as such I may even be able to implement that.

Most exciting to me was the ability to use the calender to add everything you would need by that date. I hate going grocery shopping, and being able to pull up a list and tell an AI to push out my request stock level a week or month, and never require me to build the list myself would be amazing.  While I did not succeed in bringing this to life, I had many things I loved that I also believe would be trivial to add, such as the mentioned future lists and even (hard) have it either buy and send items to the user, or almost as good, map out your grocrey store and use a greedy algorithm to chart a path to each item so I could spend the least amount of time in my Walmart. That would be absolutely amazing.


