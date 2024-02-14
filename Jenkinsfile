
pipeline {
    agent any
    environment {
        AWS_DEFAULT_REGION = 'ap-south-1'
        AWS_USER_NAME = 'Shraddha'
        oldAccessKeyId = ''
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
                            env.oldAccessKeyId = listKeys.split('"AccessKeyId": "')[1].split('"')[0]
                            println(env.oldAccessKeyId)
                            if (!env.oldAccessKeyId) {
                                error("Failed to retrieve old access key")
                            }
                        }

                  }
            }
        }
    }
}
        
