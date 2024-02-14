import com.cloudbees.plugins.credentials.impl.UsernamePasswordCredentialsImpl
import jenkins.model.Jenkins

def updateCredential = { id, old_access_key, new_access_key, new_secret_key ->
    println "Running updateCredential: (\"$id\", \"$old_access_key\", \"$new_access_key\", \"************************\")"
    def creds = com.cloudbees.plugins.credentials.CredentialsProvider.lookupCredentials(
        com.cloudbees.jenkins.plugins.awscredentials.BaseAmazonWebServicesCredentials.class,
        jenkins.model.Jenkins.instance
    )

    def c = creds.findResult { it.id == id && it.accessKey == old_access_key ? it : null }

    if ( c ) {
        println "found credential ${c.id} for access_key ${c.accessKey}"
		println c.class.toString()
        def credentials_store = jenkins.model.Jenkins.instance.getExtensionList(
          'com.cloudbees.plugins.credentials.SystemCredentialsProvider'
        )[0].getStore()
		 def tscope = c.scope as com.cloudbees.plugins.credentials.CredentialsScope
         def result = credentials_store.updateCredentials(
           com.cloudbees.plugins.credentials.domains.Domain.global(),
           c,
           new com.cloudbees.jenkins.plugins.awscredentials.AWSCredentialsImpl(tscope, c.id, new_access_key, new_secret_key, c.description, null, null)
         )

        if (result) {
            println "password changed for id: ${id}"
        } else {
            println "failed to change password for id: ${id}"
        }
    } else {
      println "could not find credential for id: ${id}"
    }
}
pipeline {
    agent any
    environment {
        AWS_DEFAULT_REGION = 'ap-south-1'
        AWS_USER_NAME = 'Shraddha'
        creds = 'shraddha-creds'
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
                                updateCredential(env.creds, oldAccessKeyId, newAccessKeyId, newSecretAccessKey)
                            }
                            if (newAccessKeyId) {
                              sh "aws iam delete-access-key --user-name ${AWS_USER_NAME} --access-key-id ${oldAccessKeyId}"
                           }
                  }
            }
        }
    }
}
}
        
