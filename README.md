## My Release

This repository is my version of the Server Deployment and Containerization Project (https://github.com/udacity/cd0157-Server-Deployment-and-Containerization).
I recorded my development through commits so that you can follow along.

Please let me know if you find any issues with this project.


## References

During the development, I used the following references to build the Coffee Shop app:

https://learn.udacity.com/nanodegrees/nd0044/
https://jwt.io/
https://shivamarora.medium.com/a-guide-to-manage-your-environment-variables-in-a-better-way-using-direnv-2c1cd475c8e
https://direnv.net/
https://pypi.org/project/python-dotenv/
https://scottspence.com/posts/notes-on-direnv


# Deploying a Flask API

In this project, I containerized and deployed a Flask API to a Kubernetes cluster using Docker, AWS EKS, CodePipeline, and CodeBuild.

The Flask app used for this project consists of a simple API with three endpoints:

- `GET '/'`: This is a simple health check, which returns the response 'Healthy'. 
- `POST '/auth'`: This takes an email and password as JSON arguments and returns a JWT based on a custom secret.
- `GET '/contents'`: This requires a valid JWT and returns the unencrypted contents of that token. 

The app relies on a secret set as the environment variable `JWT_SECRET` to produce a JWT. The built-in Flask server is adequate for local development but not production, so we used the production-ready [Gunicorn](https://gunicorn.org/) server when deploying the app.



## Prerequisites

* Docker Desktop - Installation instructions for all OSes can be found <a href="https://docs.docker.com/install/" target="_blank">here</a>.
* Git: <a href="https://git-scm.com/downloads" target="_blank">Download and install Git</a> for your system. 
* Code editor: You can <a href="https://code.visualstudio.com/download" target="_blank">download and install VS code</a> here.
* AWS Account
* Python version between 3.7 and 3.9. Check the current version using:
```bash
#  Mac/Linux/Windows 
python --version
```
You can download a specific release version from <a href="https://www.python.org/downloads/" target="_blank">here</a>.

* Python package manager - PIP 19. x or higher. Python 3 >=3.4 has PI installed and downloaded from python.org. However, you can upgrade to a specific version, say 20.2.3, using the command:
```bash
#  Mac/Linux/Windows. Check the current version
pip --version
# Mac/Linux
pip install --upgrade pip==20.2.3
# Windows
python -m pip install --upgrade pip==20.2.3
```
* Terminal
   * Mac/Linux users can use the default terminal.
   * Windows users can use either the GitBash terminal or WSL. 
* Command line utilities:
  * AWS CLI installed and configured using the 'aws configure' command. Another necessary configuration is the region. Do not use the us-east-1 because the cluster creation may fail mostly in us-east-1. Let's change the default region to:
  ```bash
  aws configure set region us-east-2  
  ```
 Note: I have created all my resources in a single region. 
  * EKSCTL installed in your system. Follow the instructions [available here](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html#installing-eksctl) or <a href="https://eksctl.io/introduction/#installation" target="_blank">here</a> to download and install `eksctl` utility. 
  * The KUBECTL installed in your system. Installation instructions for kubectl can be found <a href="https://kubernetes.io/docs/tasks/tools/install-kubectl/" target="_blank">here</a>. 


## Project setup

1. Relevant Files
```
├── Dockerfile 
├── README.md
├── aws-auth-patch.yml
├── buildspec.yml
├── ci-cd-codepipeline.cfn.yml
├── iam-role-policy.json
├── main.py
├── requirements.txt
├── simple_jwt_api.yml
├── test_main.py
└── trust.json 
```

     
## Achievements

1. Dockerfile for a simple Flask API
2. Container built and tested locally
3. EKS cluster
4. Secret stored using AWS Parameter Store
5. CodePipeline pipeline triggered by GitHub checkins
6. CodeBuild stage, which will build, test, and deploy my code
