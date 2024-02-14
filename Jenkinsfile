
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
                            oldAccessKeyId = listKeys.split('"AccessKeyId": "')[1].split('"')[0]
                            println(oldAccessKeyId)
                            if (!oldAccessKeyId) {
                                error("Failed to retrieve old access key")
                            }
                           def newAccessKeyOutput = sh (script: "aws iam create-access-key --user-name ${AWS_USER_NAME} --region ${AWS_DEFAULT_REGION}", returnStdout: true).trim()
                           println(newAccessKeyOutput)
                           def newAccessKeyId = newAccessKeyOutput.split('"AccessKeyId": "')[1].split('"')[0]
                           println(newAccessKeyId)
                           def newSecretAccessKey = newAccessKeyOutput.split('"SecretAccessKey": "')[1].split('"')[0]
                           println(newSecretAccessKey)
                           if (newAccessKeyId) {
                              sh "aws iam delete-access-key --user-name ${AWS_USER_NAME} --access-key-id ${newAccessKeyId}"
                           }
                  }
            }
        }
    }
}
        
