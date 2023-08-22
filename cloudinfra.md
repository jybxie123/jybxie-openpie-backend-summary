# CloudInfra
This is the module that I was responsible for during my last days in openpie.
As a fresh man in this field, this is the biggest system I have been in.
Over 40 thousand lines of code were written. And strong enough for extension.

### Features
It supports declarative programming.
It can help to apply for resources on different cloud service. For example, alicloud and AWS.
It can track the process of application, and deployment. 
If there is an error, this service will help to figure it out and tell the customer that somthing goes wrong.

### structure
#### declarative programming
This is the core feature of this service. With the help of cloud servive provider's api, I can easily use different api to describe the deploying progress. But my goal is to design something stable, strong like terraform. And I read some core code of this open-source project. I decided to use similar structure to build my cloudinfra service.

#### code structure
I divided my features and called functions into different parts. And finally I got a level-separated structure:
**App level:**
Integrate the main api, and explode them to the front end.
To simplify the front end's logistics, we use different structures to describe front-end data and backend data. And this level helps to convert front-end types into backend ones.
**Template level:**
This level helps to check and fill the templates that below levels will need. The input of this level is backend-type data. After feeding data into this level, it will output a completed template with all important information that we need during the requirement. What's more, this level will help to check if there is some mistake in the input. For example, the customer may input some wrong parameters for some resources. This level will try to check these kind of mistakes and return error to the front end to inform the user.
**Model level:**
This level is used to store data into database, in order to keep data persistence. And we will load the newest checkpoint to database so that we can reload data whenever the process are crashed.
**Plan level:**
This level is used to generate plans for cloudinfra. With declarative programming mentioned above, we can easily declare some devices and ask cloudinfra to apply these resources from the cloud service provider automatically.
The first step to apply for resources, is to generate a application plan.
This plan is actually the true progress that we run during this application.
In this level, cloudinfra will get the information stored in the completed template, and will generate a list of plans to apply for the required resources. For example, if we write a virtual machine and we write down some required parameter into the cloudinfra, it will generate a creation plan to inform the apply level that we will apply for a virtual machine with these parameters, and it will still generate some post-apply plans to inform the post-apply level that this kind of resource have some interesting relationships with other resources. In fact, cloudinfra will generate hundreds of plans here to ensure that all the resource we need is planned correctly and all the relationships between different resources are planned correctly.
What's more, this level will also check for some vulnerabilities that may only be discovered at runtime. Because this level is seen as a dry-run step and we can actually check as many possible errors as we can in this level.
**Apply level:**

**Repair level:**


