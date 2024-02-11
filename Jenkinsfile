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

pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Hello world!' 
                sh 'pwd'
                sh 'ls'
                sh 'docker ps -a || true'
                script {
                    withPythonEnv('python3') {
                        sh 'python3 --version'
                        sh 'python3 -m pip install --upgrade pip'
                        sh 'pip install -r requirements.txt'
                        sh 'pip freeze'
                    }
                }
            }
        }
        stage('Test') {
            node {
                def data = [
                        name: 'John',
                        age: 30,
                        city: 'New York'
                    ]

                // Convert Groovy object to JSON string
                def jsonString = JsonOutput.toJson(data)
            }
            steps {
                // Print JSON string
                echo "JSON data: ${jsonString}"
            }
        }
    }
}