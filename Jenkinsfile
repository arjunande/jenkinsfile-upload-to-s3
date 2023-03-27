pipeline {
    agent any
 
    environment{
        aws_credential = "aws-cred"
        imagename = "openjdk:8-jdk-alpine"
        api_imagename = "my-api"
        auth_imagename = "my-auth"
        bucket = "MyBucke-tName"
        region = "ap-northeast-1"
        webHook_url = "18.179.20.155:8080"
     api_res_url = "https://${bucket}.s3.${region}.amazonaws.com/${TAG_NAME}/${api_imagename}-${TAG_NAME}.tar.gz"
     auth_res_url = "https://${bucket}.s3.${region}.amazonaws.com/${TAG_NAME}/${auth_imagename}-${TAG_NAME}.tar.gz"
     notify_text = "image upload to s3 <br>${api_imagename}: <${api_res_url}><br> ${auth_imagename}: <${auth_res_url}><br>tag by ${TAG_NAME}"
        
    }

    stages {
        stage('gitclone') {                                                          
            steps {
            git url:"https://github.com/arjunande/jenkinsfile-upload-to-s3.git"
            }
        }
     }                                                                            
     stage("Zip"){
            steps{
                sh "mkdir ${TAG_NAME}"
                dir("${TAG_NAME}"){
                    sh "docker save ${api_imagename}:latest > ${api_imagename}-${TAG_NAME}.tar.gz"
                    sh "docker save ${auth_imagename}:latest > ${auth_imagename}-${TAG_NAME}.tar.gz"
                }
            }
     } 
 
        stage("Upload"){
            steps{
                withAWS(region:"${region}", credentials:"${aws_credential}){
                    s3Upload(file:"${TAG_NAME}", bucket:"${bucket}", path:"${TAG_NAME}/")
                }
                    
            }                                                            
        }
}

            
