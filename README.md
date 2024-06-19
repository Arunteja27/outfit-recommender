# outfit-recommender

SmartWardrobe: Your Personal AI Outfit Assistant
Welcome to SmartWardrobe! This project is a SaaS application designed to help users decide what outfit to wear based on their current selection of clothes, the weather, and their daily routine. SmartWardrobe leverages the power of machine learning to understand users' wardrobes, analyze clothing colors, and determine the suitability of clothes for different weather conditions and activities.

Key Features
-Personalized Outfit Recommendations: Get outfit suggestions tailored to your unique style and wardrobe.
-Weather-Aware: SmartWardrobe considers the current weather to suggest appropriate attire.
-Activity-Based Suggestions: Receive recommendations based on your daily schedule and activities.
-Style and Practicality: Balances fashion sense with functionality to ensure you look good and feel comfortable.
-Machine Learning Insights: Utilizes advanced algorithms to learn your clothing preferences and improve over time.

How It Works
Wardrobe Analysis: Upload your wardrobe to SmartWardrobe. Our machine learning models will analyze your clothes, understanding their colors, styles, and suitable conditions.
Weather Integration: Sync with real-time weather data to get accurate recommendations based on current conditions.
Activity Planning: Input your daily routine, and SmartWardrobe will suggest outfits suitable for each activity.
Continuous Learning: The more you use SmartWardrobe, the better it gets at understanding your preferences and suggesting outfits that match your style and needs.

Why SmartWardrobe?
In today's fast-paced world, deciding what to wear can be a daunting task. SmartWardrobe simplifies this process by combining style and practicality, ensuring you are always dressed appropriately for the weather and your activities. Whether you're heading to a business meeting, hitting the gym, or enjoying a casual day out, SmartWardrobe has got you covered.

Getting Started
Sign Up: Create an account on our platform.
Upload Your Wardrobe: Add photos and details of your clothes.
Set Preferences: Configure your style preferences and daily activities.
Get Recommendations: Start receiving personalized outfit suggestions.

Technologies Used
Amazon Rekognition: For analyzing wardrobe images and 
ChatGPT: Learning user preferences and deciding outfits.
Weather API: To fetch real-time weather data.
__________: For the front-end application.
__________: For the back-end services.
Amazon DynamoDB: For storing user data and wardrobe details.

Contributing
We welcome contributions from the community! If you're interested in improving SmartWardrobe, please fork the repository and submit a pull request. For major changes, please open an issue to discuss what you would like to change.

License
This project is licensed under the MIT License. See the LICENSE file for details.

Contact
For any questions or feedback, please contact us at your-email@example.com.

Thank you for checking out SmartWardrobe! We hope this project makes your daily outfit decisions easier and more enjoyable. Happy styling!




# Phase 1 architecture

### Objective for phase 1: make sure we get some form of correct output for each tool we're using

Below is the application architecture for phase 1, you can visit the public lucid chart URL [HERE](https://lucid.app/lucidchart/0c23461d-4a12-4536-8ea7-abcecfd7a402/view).

![image](src/public/images/phase-1.png)

The generic logic flow is as follows:

1. Model uploads small dataset of images to S3
2. Rekognition processes these images in the S3 bucket and labels each one
3. Each image's S3 URL & labelling informaiton is uploaded to DynamoDB as {key : value} pairs.
4. Before model is ready to make its call to GPT, it requests specific images' labels from DynamoDB
5. These labels are sent to GPT for processing, i.e. GPT makes a recommendation on what clothes go well together
6. GPT sends insights back to Model to display on terminal

### Technologies used & their engineering justification

We used a bunch of different tools for phase 1 - **AWS S3**, **AWS Rekognition**, **AWS DynamoDB** and **OpenAI GPT**

- **AWS S3**: We need a cheap, secure and scalable tool to store our dataset's images.
  
- **AWS Rekognition**: While we could have trained our own model to accurately detail labelling info of images, it would be an expensive and time consuming process. Rekognition is pretty good at image labelling and offers 5000 free images per month in its free tier, so it fits our use case. Rekognition has awesome competitors in the image labelling space but their products could not compete with Rekognition's 5000 free images per month offering, _Welp!_ we're broke students.
  
- **AWS DynamoDB**: Our use case for DynamoDB is very simple, just `key:value` pairs of an image's S3 URL & it's labelling info from Rekognition. Due to our heavy reliance on the other AWS products above (S3 & Rekognition), having our database integrated in AWS' awesome ecosystem just makes things so much easier for us. We only have to manage 1 AWS account for most of the storage & vision analysis processing. (_Yay!_) 
  
- **OpenAI GPT**: Now, you may be asking - bozos, why couldn't you just upload images to GPT4o, it does it now! Well we _could_, but it's pretty expensive to upload and analyse images with GPT (relatively speaking). With GPT, we just send it images' labelling info (which is plain text) and it will send back it's gigabrain insights. _BUUUTT_ what about AWS Bedrock? or Google Gemini? or Meta AI? _Sigh_, so many options. To be honest, we're still in Phase 1 of this project, we might pivot to another LLM for insights or train one ourselves after reflecting and analysing how each phase went! _Oooh_ AND, we were stuck whether or not to pick GPT or AWS Bedrock for insights provision, but this Medium article [here](https://medium.com/version-1/aws-bedrocks-claude-2-100k-vs-azure-openai-s-gpt-4-32k-a-comparative-analysis-96e3eb9fd05a) pretty much sealed the deal on that - GPT is better than Bedrock right now for dataset analysis. Again, our choice of LLM could change in Phase 2 and onwards.

**TODOs:** 
- [ ] Write intro to project in `README.md` 
- [ ] A small dataset should be uploaded to S3 
- [ ] Rekognition should automatically process these new images
- [ ] Rekognition should return labels for each image
- [ ] Image data stored in DynamoDB, with key = image's S3 URL and value = image's labels
- [ ] Send multiple image's label info to GPT and get some form of insight back
- [ ] Display insights on terminal
- [ ] Video demo to be published on `README.md`
- [ ] Application deployed on website  



