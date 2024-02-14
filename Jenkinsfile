
pipeline {
    agent any
    environment {
        AWS_DEFAULT_REGION = 'ap-south-1'
        AWS_USER_NAME = 'Shraddha'
    }
    stages {
        stage('Get old access key') {
            steps {
                script {
                    withCredentials([[
                       $class: 'AmazonWebServicesCredentialsBinding',
                       credentialsId: "AWS_credentials",
                       accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                       secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                    ]]) {
                            def listKeys = sh (script: "aws iam list-access-keys --user-name ${AWS_USER_NAME}", returnStdout: true).trim()
                            println(listKeys)
                            def oldAccessKeyId = listKeys.split('"AccessKeyId": "')[1].split('"')[0]
                            println(oldAccessKeyId)
                            def oldAccessKey = sh (script: "echo \$listKeys | jq -r '.AccessKeyMetadata[0]?.AccessKeyId'", returnStdout: true).trim()
                            if (!oldAccessKey) {
                                error("Failed to retrieve old access key")
                            }
                        }

                  }
            }
        }
    }
}
        
