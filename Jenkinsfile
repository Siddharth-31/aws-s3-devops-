pipeline {
    agent any

    environment {
        S3_BUCKET = 'aws-s3-devops-1'
        AWS_DEFAULT_REGION = 'ap-south-1'
    }

    stages {
        stage('Validate Website') {
            steps {
                sh '''
                    test -s index.html
                    test -s style.css
                    test -s script.js
                    echo "Website files validated."
                '''
            }
        }

        stage('Deploy to S3') {
            steps {
                withCredentials([
                    string(credentialsId: 'aws-access-key-id', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws-secret-access-key', variable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    sh '''
                        set +x
                        aws s3 cp index.html s3://$S3_BUCKET/index.html --content-type text/html
                        aws s3 cp style.css s3://$S3_BUCKET/style.css --content-type text/css
                        aws s3 cp script.js s3://$S3_BUCKET/script.js --content-type application/javascript
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Website deployed successfully to AWS S3!'
        }
    }
}
