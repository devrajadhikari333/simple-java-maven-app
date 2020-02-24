pipeline {
    agent {
        label 'maven'
    }
    tools {
        jdk 'openjdk8'
        maven 'maven'
    }
    parameters {
        choice choices: ['Dev', 'Test', 'Prod'], description: 'choosing the server', name: 'target_server'
    }
    options {
        timeout(1)
    }
    stages {
        stage('Test'){
            steps {
                sh "mvn clean test surefire-report:report-only"
                publishHTML([allowMissing: true, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'target/site', reportFiles: 'surefire-report.html', reportName: 'Test Report', reportTitles: ''])
            }            
        }
        stage('Packaging'){ 
            steps {
                sh "mvn package -DskipTests=true"
            }
            post {
                success {
                    echo "good job packaging is done" //added some comments
                }
                failure {
                    echo "failure packaging could not done"
                }
                always {
                    echo "work hard"
                }
            }
        }
        
        stage('Deploy'){
            steps{
                //sh "mail -s 'the job is waiting your approval devrajadhikari333@gmail.com'" //if user want to send email to the approval
                input message: 'Do you want me to Deploy? ', ok: 'Approve'
                script {
                    def target = getTargetIp(params.target_server)
                    sshagent(['target-dev']) {
                        sh "scp -o StrictHostKeyChecking=no target/my-app-1-RELEASE.jar ec2-user@${target}:/home/ec2-user"
                    }
                }
            }
        }
    }
    post {
        success {
            echo "good job" //added some comments
        }
        failure {
            echo "failure work hard"
        }
        always {
            echo "work hard"
        }
    }
}
def getTargetIp(target_env){
    if (target_env == "dev"){
        return "172.31.89.147"
    } else if (target_env == "Test"){
        return "0.0.0.0"
    } else if (target_env == "Prod"){
        return "1.1.1.1"
    }
}

// // node('docker'){
// //     stage('checkout'){
// //         echo "Checking the Git code"
// //         //git brach: 'docker' credentialsId: 'lokigithubapikey', url: 'https://github.com/lokeshkamalay/simple-java-maven-app.git'
// //         checkout scm
// //     }
// //     stage('Executing Test Cases'){
// //         docker.image('lokeshkamalay/batch2:maven').inside(){
// //             echo "Execuring Test Cases Started"
// //             sh "mvn clean deploy"
// //         }
// //     }
// // }




// /*node('mavenbuilds'){
//     def mvnHome = tool name: 'maven354', type: 'maven'
//     stage('checkout'){
//         echo "Checking the Git code"
//         git credentialsId: 'lokigithubapikey', url: 'https://github.com/lokeshkamalay/simple-java-maven-app.git'
//     }
//     stage('Executing Test Cases'){
//         echo "Execuring Test Cases Started"
//         sh "$mvnHome/bin/mvn clean test surefire-report:report-only"
//         archiveArtifacts 'target/**/*'
//         junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
//         publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'target/site', reportFiles: 'surefire-report.html', reportName: 'SureFireReportHTML', reportTitles: ''])
//         echo "Executing Test Cases Completed"
//     }
//     //stage('Sonar Analysis'){
//     //    sh "$mvnHome/bin/mvn sonar:sonar -Dsonar.host.url=http://aefdc217.ngrok.io -Dsonar.login=d08d80d05ae55ae9de4ca22bc2fd5140c1308ee2"
//     //}
//     stage('Packaging'){
//         echo "Preparing artifacts"
//         sh "$mvnHome/bin/mvn package -DskipTests=true"
//     }
//     stage('Push to artifactory'){
//           sh "$mvnHome/bin/mvn deploy -DskipTests=true --settings settings.xml"
//     }
//     stage('Deployments'){
//         sh 'curl http://fa1b7800.ngrok.io/artifactory/maven-local/com/mycompany/app/my-app/1-RELEASE/my-app-1-RELEASE.jar -o my-app.jar'
//         sshagent(['deployment-id']) {
//             sh 'scp -o StrictHostKeyChecking=no my-app.jar ubuntu@172.31.94.69:~/'
//         }

//     }
// }*/
