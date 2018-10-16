#!/usr/bin/env groovy

node {
    stage('checkout') {
        checkout scm
    }

        stage('check java') {
            bat "java -version"
        }

        stage('clean') {
            bat "chmod +x mvnw"
            bat "./mvnw clean"
        }

        stage('install tools') {
            bat "./mvnw com.github.eirslett:frontend-maven-plugin:install-node-and-npm -DnodeVersion=v8.11.4 -DnpmVersion=6.4.1"
        }

        stage('npm install') {
            bat "./mvnw com.github.eirslett:frontend-maven-plugin:npm"
        }

        stage('backend tests') {
            try {
                bat "./mvnw test"
            } catch(err) {
                throw err
            } finally {
                junit '**/target/surefire-reports/TEST-*.xml'
            }
        }

        stage('frontend tests') {
            try {
                bat "./mvnw com.github.eirslett:frontend-maven-plugin:npm -Dfrontend.npm.arguments='test -- -u'"
            } catch(err) {
                throw err
            } finally {
                junit '**/target/test-results/jest/TESTS-*.xml'
            }
        }

        stage('packaging') {
            bat "./mvnw verify -Pprod -DskipTests"
            archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
        }
  
}
