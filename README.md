🚀 Automated Website Deployment using Jenkins CI/CD and AWS S3

<p align="center"> <img src="https://img.shields.io/badge/AWS-S3-orange?style=for-the-badge&logo=amazons3&logoColor=white" alt="AWS S3" /> <img src="https://img.shields.io/badge/Jenkins-CI%2FCD-D24939?style=for-the-badge&logo=jenkins&logoColor=white" alt="Jenkins" /> <img src="https://img.shields.io/badge/GitHub-Version%20Control-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub" /> <img src="https://img.shields.io/badge/AWS%20CLI-Automation-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white" alt="AWS CLI" /> <img src="https://img.shields.io/badge/Ubuntu-Linux-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" alt="Ubuntu Linux" /> </p>

<p align="center"> <b>A hands-on DevOps project demonstrating automated static website deployment using Jenkins, GitHub, AWS CLI, and Amazon S3.</b> </p>

<p align="center"> 🌐 <a href="http://aws-s3-devops-1.s3-website.ap-south-1.amazonaws.com/"><b>View Live Website</b></a> &nbsp; | &nbsp; 💻 <b>Explore the Repository</b> </p>

📌 Project Overview

This project demonstrates the implementation of a CI/CD pipeline to automate the deployment of a static website to Amazon S3 using Jenkins.

The website is developed using HTML, CSS, and JavaScript, with source code maintained in GitHub. Jenkins monitors the repository for changes using Poll SCM, validates the required website files, and uses the AWS CLI to upload the website content to an S3 bucket.

The goal is to simplify website deployment, reduce manual effort, and understand how DevOps tools work together in a real-world deployment workflow.

🌐 Live Demo

🔗 Website: Visit the Live Website

Note: This is an Amazon S3 static website endpoint using HTTP. HTTPS can be configured using Amazon CloudFront.
🛠️ Technologies Used
Technology	Purpose
☁️ AWS S3	Static website hosting and file storage
🔐 AWS IAM	Identity and access management
⚙️ Jenkins	CI/CD pipeline automation
🐙 Git & GitHub	Source code management and version control
💻 AWS CLI	Automated deployment to S3
🐧 Ubuntu Linux	Deployment environment
🌐 HTML5	Website structure
🎨 CSS3	Website styling and responsive design
⚡ JavaScript	Website interactivity
🏗️ Project Architecture
             👨‍💻 Developer
                   |
                   | Push code
                   ▼
             🐙 GitHub Repository
                   |
                   | Poll SCM
                   ▼
              ⚙️ Jenkins
                   |
                   ▼
          🧪 Pipeline Validation
                   |
                   ▼
             💻 AWS CLI
                   |
          🔐 IAM Permissions
                   |
                   ▼
             ☁️ Amazon S3
                   |
                   ▼
            🌐 Live Website

🔄 CI/CD Workflow
Code Development: Create or update website files using HTML, CSS, and JavaScript.
Source Code Management: Push changes to the GitHub repository.
Automated Monitoring: Jenkins checks the repository for changes using Poll SCM.
Pipeline Execution: Jenkins starts the configured pipeline when changes are detected.
File Validation: The pipeline verifies that the required website files are available.
Automated Deployment: AWS CLI uploads the website files to the configured S3 bucket.
Website Hosting: Amazon S3 serves the updated static website through its website endpoint.
Verification: Open the live website to verify the deployed changes.
📂 Project Structure
aws-s3-devops-/
│
├── 📄 index.html       # Main website page
├── 🎨 style.css        # Website styling
├── ⚡ script.js        # JavaScript functionality
├── ⚙️ Jenkinsfile      # Jenkins pipeline definition
├── 🚫 .gitignore       # Files excluded from Git
└── 📘 README.md        # Project documentation

☁️ AWS Configuration
Configuration	Details
AWS Service	Amazon S3
AWS Region	ap-south-1 (Mumbai)
S3 Bucket	aws-s3-devops-1
Hosting Type	S3 Static Website Hosting
Deployment Tool	AWS CLI
CI/CD Tool	Jenkins
Source Control	GitHub
Operating System	Ubuntu Linux
⚙️ Jenkins Pipeline

The Jenkinsfile defines the automated deployment process.

The pipeline is designed to:

Retrieve source code from GitHub.
Validate required website files.
Configure access to AWS through Jenkins Credentials.
Upload website files to Amazon S3.
Report the deployment result through the Jenkins build status.
Example Jenkins Pipeline

The following is an illustrative declarative pipeline. Update the repository URL, Jenkins credential ID, and bucket configuration to match your environment before using it.

pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'ap-south-1'
        S3_BUCKET = 'aws-s3-devops-1'
        AWS_CREDENTIALS_ID = 'aws-credentials'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'YOUR_GITHUB_REPOSITORY_URL'
            }
        }

        stage('Validate Website Files') {
            steps {
                sh '''
                    test -f index.html
                    test -f style.css
                    test -f script.js
                    echo "Website files validated successfully."
                '''
            }
        }

        stage('Deploy to Amazon S3') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: "${AWS_CREDENTIALS_ID}"
                ]]) {
                    sh '''
                        aws sts get-caller-identity

                        aws s3 sync . s3://${S3_BUCKET}/ \
                            --exclude ".git/*" \
                            --exclude "Jenkinsfile" \
                            --exclude "README.md" \
                            --exclude ".gitignore"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Website deployment completed successfully!'
        }

        failure {
            echo 'Deployment failed. Check the Jenkins console output.'
        }
    }
}


Important: This example requires the Jenkins Pipeline and AWS Credentials plugins, a compatible AWS CLI installation, and a configured AWS credentials entry named aws-credentials. Configure Poll SCM separately in the Jenkins job if you want periodic repository checks. The example is a template and has not been executed against your environment.

🔐 Security Best Practices

Security is an important part of any DevOps deployment pipeline.

🔑 Store AWS access credentials in Jenkins Credentials, not in source code.
🛡️ Follow the principle of least privilege when creating IAM policies.
🚫 Never commit AWS access keys, secret keys, or other credentials to GitHub.
🔒 Restrict deployment permissions to the intended S3 bucket and required actions.
🧹 Exclude sensitive files and unnecessary repository files from deployment.
📋 Review Jenkins console logs to ensure secrets are not exposed.
☁️ Prefer temporary credentials or IAM roles where supported by your deployment environment.
Security note: S3 static website endpoints use HTTP. For HTTPS and a production-oriented setup, consider Amazon CloudFront with an appropriate origin configuration.
🎯 Key Learnings

Through this project, I gained practical experience with:

⚙️ Creating and executing Jenkins CI/CD pipelines.
🐙 Integrating GitHub with Jenkins.
🔄 Automating deployments using AWS CLI.
☁️ Hosting static websites on Amazon S3.
🔐 Configuring AWS IAM permissions and Jenkins Credentials.
🐧 Working with Ubuntu Linux and command-line tools.
🏗️ Understanding CI/CD architecture and deployment automation.
🛡️ Applying security best practices to cloud deployments.
🚀 Future Improvements
🔔 Integrate GitHub Webhooks for event-driven builds.
🧪 Add automated HTML validation and testing.
🌍 Configure Amazon CloudFront and HTTPS.
🔔 Add email or messaging notifications for build results.
🏗️ Automate AWS infrastructure provisioning with Terraform.
📊 Add monitoring, logging, and deployment reporting.
🔄 Implement safer releases with versioning and rollback support.
👨‍💻 About Me

Siddharth
Aspiring DevOps Engineer | AWS Cloud | Jenkins CI/CD | Linux | Git & GitHub

I am passionate about cloud computing, DevOps practices, CI/CD automation, and building practical projects that improve software delivery.

This project represents my hands-on learning journey in automating deployments and working with AWS cloud services.

🤝 Connect With Me
💼 LinkedIn: Add your LinkedIn profile URL
🐙 GitHub: Explore my GitHub profile

<p align="center"> ⭐ If you find this project useful, consider giving the repository a star! </p>

<p align="center"> <b>Built with ❤️ by Siddharth</b> </p>
