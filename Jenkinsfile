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
def screenshotsStash = "screenshots"

fabric8EETestNode {
  try {
    container(name: 'test') {
      try {
        sh """
        export TARGET_URL="https://openshift.io"
        export TEST_PLATFORM="osio"
        
        /test/ee_tests/entrypoint.sh
    """
      } finally {

        echo ""
        echo ""
        echo "functional_tests.log:"
        sh "cat /test/ee_tests/functional_tests.log"

        sh "mkdir -p screenshots"
        sh "cp -r /test/ee_tests/target/screenshots/* screenshots"

        stash name: screenshotsStash, includes: "screenshots/*"
      }
    }
  } finally {

    echo "unstashing ${screenshotsStash}"
    unstash screenshotsStash

    echo "how lets try archive them: ${screenshotsStash}"
    try {
      archiveArtifacts artifacts: 'screenshots/*'
    } catch (e) {
      echo "could not find: screenshots* ${e}"
    }
  }
}

