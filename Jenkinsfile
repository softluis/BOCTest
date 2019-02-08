def FAILED_STAGE
pipeline{
    agent none
    stages{
        stage('Pipeline'){
            parallel{
                stage('Android'){
                    agent any
                    stages{
                        stage('Git Checkout'){
                            steps{
                                script{
                                    FAILED_STAGE=env.STAGE_NAME
                                }
                                echo "Checking out git repo on ---- ${NODE_NAME}"
                                checkout scm
                            }
                        }
                        stage('Build'){
                            steps{
                                script{
                                    FAILED_STAGE=env.STAGE_NAME
                                }
                                echo "Analising code on ---- ${NODE_NAME}"
                                //Navigate to project folder(assemble -> assembleDebug || assembleRelease (This is optimized) || build)
                                //COMMAND: ./gradlew assembleRelease
                            }
                        }
                        stage('SonarQube analysis'){
                            environment{
                                scannerHome = tool 'Scanner';
                            }
                            steps{
                                script{
                                    FAILED_STAGE=env.STAGE_NAME
                                }
                                echo "SonarQube analysis"
                                withSonarQubeEnv('SonarServer') {
                                    echo 'sh "\"${scannerHome}/bin/sonar-scanner\""'
                                }
                                sleep 5
                            }
                        }
                        /*
                        stage("SonarQube Quality Gate") { 
                            steps{
                                script{
                                    FAILED_STAGE=env.STAGE_NAME
                                }
                                echo "SonarQube Quality Gate"
                                timeout(time: 1, unit: 'MINUTES') {  
                                    waitForQualityGate abortPipeline: true
                                }
                            }
                        }
                        */
                        //For this stage use Jenkins Plugin for IBM AppScan and the Jenkins Syntax Generator to grab the resulting code for pipeline
                        stage("Security Analysis with AppScan"){
                            steps{
                                script{
                                    FAILED_STAGE=env.STAGE_NAME
                                }
                                echo "Security Analysis on iOS"
                                /* Example
                                script{
                                    step([$class: 'AppScanStandardBuilder', additionalCommands: '', authScanPw: '',
                                    authScanUser: '', includeURLS: '', installation: 'AppScan Standard Default',
                                    pathRecordedLoginSequence: '', policyFile: '', reportName: '', startingURL: 'demo.testfire.net'])
                                }
                                */
                            }
                        }
                        stage("Test"){
                            steps{
                                script{
                                    FAILED_STAGE=env.STAGE_NAME
                                }
                                echo "Automated Testing on iOS"
                                /* Example
                                    runInCloud(
                                    // parameters to start test with...
                                        cloudUrl: "https://cloud.bitbar.com",
                                        credentialsId: "my-bitbar-cloud-credentials-01",
                                        projectId: "12345",
                                        deviceGroupId: "110",
                                        appPath: "/path/to/app.apk",
                                        testPath: "/path/to/tests.zip",
                                        // ... don't just start test, also wait for results!
                                        waitForResults: true,
                                        testRunStateCheckMethod: "API_CALL",
                                        resultsPath: "results"
                                    )
                                */
                            }
                        }
                        stage('Deploying'){
                            steps{
                                script{
                                    FAILED_STAGE=env.STAGE_NAME
                                }
                                echo "Deploying code on ---- ${NODE_NAME}"
                                //bat 'testfairy-upload-android-windows.bat [PROJECT_APK]'
                            }
                        }
                    }
                    post{
                        always{
                            deleteDir()
                        }
                        success{
                            slackSend color: 'good', message: "SUCCESS: ${currentBuild.fullDisplayName}\nANDROID"
                        }
                        failure{
                            slackSend color: 'danger', message: "FAILURE: ${currentBuild.fullDisplayName}\nANDROID\nFailed On: ${FAILED_STAGE}"
                        }
                    }
                }
                stage('iOS'){
                    agent{
                        label 'MAC_Agent'
                    }
                    stages{
                        stage('Git Checkout'){
                            steps{
                                script{
                                    FAILED_STAGE=env.STAGE_NAME
                                }
                                echo "Checking out git repo on ---- ${NODE_NAME}"
                                checkout scm
                            }
                        }
                        stage('Build'){
                            steps{
                                script{
                                    FAILED_STAGE=env.STAGE_NAME
                                }
                                echo "Build Step for iOS"
                                //Navigate to project folder (Note: it's where the <Name>.xcodeproj resides)
                                //The command below will list all schemes, we then need to choose one
                                //COMMAND: sh 'xcodebuild -list -project <NAME>.xcodeproj/'
                                //We then choose a scheme and we run it
                                //COMMAND: sh 'xcodebuild -scheme <SCHEME_NAME> build'
                                //OR
                                //COMMAND: sh 'xcodebuild -scheme <SCHEME_NAME> -workspace <WORKSPACE_NAME>/ build'
                            }
                        }
                        stage('SonarQube analysis'){
                            environment{
                                scannerHome = tool 'Scanner';
                            }
                            steps{
                                script{
                                    FAILED_STAGE=env.STAGE_NAME
                                }
                                echo "SonarQube analysis"
                                withSonarQubeEnv('SonarServer') {
                                    echo 'sh "\"${scannerHome}/bin/sonar-scanner\""'
                                }
                                sleep 5
                            }
                        }
                        stage("SonarQube Quality Gate") { 
                            steps{
                                script{
                                    FAILED_STAGE=env.STAGE_NAME
                                }
                                echo "SonarQube Quality Gate"
                                "timeout(time: 1, unit: 'MINUTES') {"  
                                    "waitForQualityGate abortPipeline: true"
                                "}
                            }
                        }
                        //For this stage use Jenkins Plugins for IBM AppScan and the Jenkins Syntax Generator to grab the resulting code for pipeline
                        stage("Security Analysis with AppScan"){
                            steps{
                                script{
                                    FAILED_STAGE=env.STAGE_NAME
                                }
                                echo "Security Analysis on iOS"
                                /* Example
                                script{
                                    step([$class: 'AppScanStandardBuilder', additionalCommands: '', authScanPw: '',
                                    authScanUser: '', includeURLS: '', installation: 'AppScan Standard Default',
                                    pathRecordedLoginSequence: '', policyFile: '', reportName: '', startingURL: 'demo.testfire.net'])
                                }
                                */
                            }
                        }
                        stage("Test"){
                            steps{
                                script{
                                    FAILED_STAGE=env.STAGE_NAME
                                }
                                echo "Automated Testing on iOS"
                                /* Example
                                    runInCloud(
                                    // parameters to start test with...
                                        cloudUrl: "https://cloud.bitbar.com",
                                        credentialsId: "my-bitbar-cloud-credentials-01",
                                        projectId: "12345",
                                        deviceGroupId: "110",
                                        appPath: "/path/to/app.apk",
                                        testPath: "/path/to/tests.zip",
                                        // ... don't just start test, also wait for results!
                                        waitForResults: true,
                                        testRunStateCheckMethod: "API_CALL",
                                        resultsPath: "results"
                                    )
                                */
                            }
                        }
                        stage('Deploying'){
                            steps{
                                script{
                                    FAILED_STAGE=env.STAGE_NAME
                                }
                                echo "${UNKNOWN_VAR}"
                                echo "Deploying code on ---- ${NODE_NAME}"
                                //sh 'testfairy-uploader.sh [PROJECT_APK]'
                            }
                        }
                    }
                    post{
                        always{
                            deleteDir()
                        }
                        success{
                            slackSend color: 'good', message: "SUCCESS: ${currentBuild.fullDisplayName}\niOS"
                        }
                        failure{
                            slackSend color: 'danger', message: "FAILURE: ${currentBuild.fullDisplayName}\niOS\nFailed On: ${FAILED_STAGE}"
                        }
                    }
                }
            }
        }
    }
    post{
        success{
            slackSend color: 'good', message: "SUCCESS: ${currentBuild.fullDisplayName}\nInfo: ${env.BUILD_URL}"
        }
        failure{
            slackSend color: 'danger', message: "FAILURE: ${currentBuild.fullDisplayName}\nInfo: ${env.BUILD_URL}"
        }
    }
}
