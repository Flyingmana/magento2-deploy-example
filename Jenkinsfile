#!groovy​
pipeline {
  agent any
  stages {
    stage('fetch dependencies') {
        steps {
            checkout scm
            sh 'php --version'
            php bin/install_main.php
        }
    }
    stage('placeholder') {
        steps {
            sh 'php ./project-community-edition/bin/magento'
        }
    }
  }
}
