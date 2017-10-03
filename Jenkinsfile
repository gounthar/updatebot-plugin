#!/usr/bin/groovy
/**
 * Copyright (C) 2015 Red Hat, Inc.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *         http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */
@Library('github.com/fabric8io/fabric8-pipeline-library@master')
def utils = new io.fabric8.Utils()
clientsTemplate{
  mavenNode {
    ws{
      checkout scm
      readTrusted 'release.groovy'
      if (utils.isCI()){
        echo 'Running CI pipeline'
        container(name: 'maven') {
          sh 'mvn clean install'
        }
      } else if (utils.isCD()){
        echo 'Running CD pipeline'
        sh "git remote set-url origin git@github.com:fabric8-updatebot/updatebot-pipeline.git"

        def pipeline = load 'release.groovy'

        stage 'Stage'
        def stagedProject = pipeline.stage()

        stage 'Promote'
        pipeline.release(stagedProject)

/*
        stage 'Update downstream dependencies'
        pipeline.updateDownstreamDependencies(stagedProject)
*/
      }
    }
  }
}
