/*
 * Licensed to the Apache Software Foundation (ASF) under one or more
 * contributor license agreements.  See the NOTICE file distributed with
 * this work for additional information regarding copyright ownership.
 * The ASF licenses this file to You under the Apache License, Version 2.0
 * (the "License"); you may not use this file except in compliance with
 * the License.  You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 *
 */

pipeline {
    agent {
        label 'ubuntu'
    }

    tools {
        jdk 'JDK 1.8 (latest)'
    }

    options {
        buildDiscarder(logRotator(
            numToKeepStr: '30',
        ))
        timestamps()
        skipStagesAfterUnstable()
        timeout time: 30, unit: 'MINUTES'
    }

    stages {
        stage('SCM Checkout') {
            steps {
                deleteDir()
                checkout scm
            }
        }

        stage('Check environment') {
            steps {
                sh 'env'
                sh 'pwd'
                sh 'ls'
                sh 'git status'
            }
        }

        stage('Run install') {
            steps {
                sh './mvnw org.jacoco:jacoco-maven-plugin:0.8.3:prepare-agent clean install org.jacoco:jacoco-maven-plugin:0.8.3:report coveralls:report --quiet --batch-mode -nsu'
            }
        }

        stage('Run install') {
            steps {
                sh './mvnw javadoc:javadoc -Dmaven.test.skip=true --quiet'
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/*.xml'
            deleteDir()
        }
    }
}