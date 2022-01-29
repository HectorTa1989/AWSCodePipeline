# AWSCodePipeline
Creating a CI/CD Pipeline with AWS CodePipeline

Preface:
CI/CD= Continuous Integration and Continuous Delivery are a set of operating principles in application development that allow software development teams to deliver frequent and reliable code changes more easily. This is through a “pipeline”.
CI/CD is a practice of DevOps and automates the deployment process for software.
Objective:
To create a CI/CD pipeline using AWS CodePipeline. I am going to use an angular application in Github to perform this simple project.
Disclaimer: This project is for demonstrating AWS CodePipeline only. The application/code part of this project was already prepared.
Step 1: Create an S3 bucket
The application will need to be hosted in S3. If you go to S3 in the AWS console and click “create bucket” you will be able to set up your bucket. You will need to create a globally unique name for it and se the correct region. You will also need to uncheck “block all public access”. After you create the bucket, you will need to make some configuration changes.
Go to the properties tab of the bucket and click on the static webhosting box and select “use this bucket to host a website”.
Now go to the permissions tab and click on bucket policy, you will need to change the policy of the bucket to make the S3 bucket public. Use this code below:
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/*"
    }
  ]
}
Step 2: Create AWS CodePipeline
Go to CodePipeline in the AWS console and clicke on “Create a pipeline”
Create a Pipeline name
Select New service role. This creates an IAM role specifically to access the pipeline.
Click Next
Step 3: Select a source provider
You will need to provide a source provider. This just means where is the source code currently hosted. In this case we will be selecting Github and then clicking on “Connect to Github” and insert our credentials to authorize it. You will also need to specific your repository and branch.
Select GitHub webhooks as the change detection option. What this does is that every time a change is recorded in the repository, the pipeline will automatically detect the change and update the web application accordingly.
Step 4: Create CodeBuild
We are going to use AWS CodeBuild as the build provider.
Use the following configurations:

![image](https://user-images.githubusercontent.com/31132150/151681483-eabad7f4-6d56-4860-a8c3-23b7c65165f1.png)


Step 5: Deploy the application
Select AWS S3 as your deploy provider and select the appropriate region and bucket name. Make sure to select “extract file before deploy”. You will be able to launch the pipeline afterwards and should receive a screen like this:

![image](https://user-images.githubusercontent.com/31132150/151681494-bfed7a8a-9c01-4f88-b1e9-a825c0fce53e.png)


Step 6: Test your Pipeline
To test to ensure the pipeline is working correctly, I am going to make some changes to the GitHub code for our web page. Before that, we will look at what the code was before we changed it and then what it looks like afterwards. Open up your webpage using the link from the S3 bucket.
Before the change:

![image](https://user-images.githubusercontent.com/31132150/151681504-935984e7-cd0f-4e9b-ba37-083ac7f7cc90.png)


Go ahead and make the changes in the Github repository:

![image](https://user-images.githubusercontent.com/31132150/151681515-2a1f9b54-9e0a-420f-ab4c-d307a48b9e3d.png)


In this case I changed line 4 from “Stay Hungry” to “Stay Positive”
Once the change was completed, re-deploy the application in AWS CodePipeline. After several minutes, it will detect the change and your code will reflect the update made in GitHub:

![image](https://user-images.githubusercontent.com/31132150/151681519-1f3ccb6d-b097-4675-bbeb-8f4f3baee0dc.png)

Once the change was completed, re-deploy the application in AWS CodePipeline. After several minutes, it will detect the change and your code will reflect the update made in GitHub:

Project completed!
