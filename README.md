## Automating Security and Compliance Testing of Infrastructure-as-Code for DevSecOps

```
AWS Security Week SF
May 2018

Lab by:
Alex Corstorphine - senior cloud solutions architect, Dome9 Security
Roy Feintuch - co-founder & CTO, Dome9 Security

www.dome9.com
```
### What is this lab all about?
In this lab we'll be creating a DevSecOps pipeline in which users will commit their application/infrastructure CFT code into an S3 bucket, which will then be picked up by an AWS `CodePipeline` which will run a `Lambda` function to validate the CFT according to our business and security rules. If all is well, the pipeline will continue to depoy the CFT as a production stack.
```
Your CFT code -> S3 bucket -> Verification Lambda Function -> CloudFormation Deploy -> a deployed app / infrastructure
                |----------------------- our CodePipeline ------------------------|
```
We'll provide you with all the plumbing to implement the pipeline, while you will use your coding skills to implement a the logic inside the validation function Lambda that will decide if to continue the pipeline or stop it.


### Setup Instructions
1. Clone this repo or download from github
1. Create your own S3 bucket in the desired region and **enable versioning** on the bucket 
1. Review the push___.sh scripts and **UPDATE** your S3 bucket name
1. Run `./push_validation_lambda.sh` to upload the pipline validation code to S3
1. Create new stack in the CloudFormation service using `pipeline/pipeline-cft.json`. Fill out the parameters (bucket and email address), and click next.next...
1. View your brand new pieline on AWS CodePipline page (it should fail as we didn't push our app yet)
1. Test your pipeline by uploading your first 'app' `./push_app.sh exercises/level1/valid` (it should pass because we haven't added any checks to Lambda)

### Test Instructions
Once you have a working pipeline, start following the exercises in the `exercises` folder. <br/>

You can implement your validation logic directly within the AWS Lambda console. Your Lambda name should be something like: <stackname>-CFTValidateLambda-<random> and search for the `evaluate_template` function. <br/>
In a real situation we would be working on these Lambda functions locally, and committing them to a source control system, but in this case we can quickly hack something in the AWS Lambda editor.<br/>

For each level test a non-valid sample (which should fail) and a valid one which should pass.
```bash
./push_app.sh exercises/level1/nonvalid
# now look at your pipeline. If you implemented your Lambda corectly, the piepline should fail

./push_app.sh exercises/level1/valid
# now review your pipeline which should succeed. The new stack should be deployed now

```
