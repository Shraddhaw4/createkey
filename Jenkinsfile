
pipeline {
    agent any
    stages {
        stage('Rotate AWS Access keys') {
            steps {
                script {
                    def iamUser = 'Shraddha'
                    def awsRegion = 'ap-south-1'
 
                    def newAccessKeyOutput = sh (script: "aws iam create-access-key --user-name ${iamUser} --region ${awsRegion}", returnStdout: true).trim()
                    println(newAccessKeyOutput)
                    def newAccessKeyId = newAccessKeyOutput.split('"AccessKeyId": "')[1].split('"')[0]
                    println(newAccessKeyId)
                    def newSecretAccessKey = newAccessKeyOutput.split('"SecretAccessKey": "')[1].split('"')[0]
                    println(newSecretAccessKey)
                    }
                }
            }
        }
    }
