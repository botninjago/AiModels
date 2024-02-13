/*
    Jenkinsfile for running workflow of AI models
*/

/* Libs imported
FilenameUtils provides various methods for manipulating and working with file and directory names.
Use FilenameUtils in a Jenkinsfile when you need to manipulate file paths or filenames, such as extracting file extensions, checking if a file exists, or resolving relative paths to absolute paths.
Using FilenameUtils, you can perform these operations in a platform-independent manner, as it handles differences in file separators (/ vs \) between operating systems automatically.

JsonOutput class from the Groovy JSON support library. 
This class provides utilities for converting Groovy objects to JSON format.
*/
import org.apache.commons.io.FilenameUtils  
import groovy.json.JsonOutput

def show_node_info() {
    sh """
        echo "NODE_NAME = \$NODE_NAME" || true
        lsb_release -sd || true
        uname -r || true
        cat /sys/module/amdgpu/version || true
        ls /opt/ -la || true
    """
}

def clean_up_docker() {
    sh 'docker ps -a || true' // "|| true" suppresses errors
    sh 'docker kill $(docker ps -q) || true'
    sh 'docker rm $(docker ps -a -q) || true'
    sh 'docker rmi $(docker images -q) || true'
    sh 'docker system prune -af --volumes || true'
}

def clean_up_docker_container() {
    sh 'docker ps -a || true' // "|| true" suppresses errors
    sh 'docker kill $(docker ps -q) || true'
}

def doSteps() {
    //show node information
    show_node_info()

    // Clean before build
    cleanWs()

    // We need to explicitly checkout from SCM here
    checkout scm    

    withPythonEnv('python3') {
        // install requirements
        sh 'python3 -m pip install --upgrade pip || true'
        sh 'python3 -m pip install -r requirements.txt || true'
    }

    clean_up_docker()
}

pipeline {
    agent any
    stages {
        // stage('Checkout') {
        //     steps {
        //         checkout scm
        //     }
        // }
        // stage('Build') {
        //     steps {
        //         echo 'Hello world!' 
        //         sh 'pwd'
        //         sh 'ls'
        //         sh 'docker ps -a || true'
        //         script {
        //             withPythonEnv('python3') {
        //                 sh 'python3 --version'
        //                 sh 'python3 -m pip install --upgrade pip'
        //                 sh 'pip install -r requirements.txt'
        //                 sh 'pip freeze'
        //             }
        //         }
        //     }
        // }
        // stage('Test') {
        //     steps {
        //         script{
        //             def data = [
        //                     name: 'John',
        //                     age: 30,
        //                     city: 'New York'
        //                 ]

        //             // Convert Groovy object to JSON string
        //             def jsonString = JsonOutput.toJson(data)        
                    
        //             // Print JSON string
        //             echo "JSON data: ${jsonString}"            
        //         }

        //     }
        // }
        stage('Models') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-user', usernameVariable: 'hub_user', passwordVariable: 'hub_password')]) {
                        sh 'docker login -u $hub_user -p $hub_password'
                        doSteps()
                    }
                }
            }
        }
    }
}