pipeline {
  agent any
  stages {
    stage('Clean') {
      parallel {
        stage('Clean') {
          steps {
            sh 'git reset --hard'
            sh 'sudo make O=out clean'
          }
        }

        stage('Rm old kernel') {
          steps {
            sh '''sudo rm -rf /boot/**5.**
sudo rm -rf /lib/modules/**'''
          }
        }

      }
    }

    stage('Kconfig') {
      steps {
        sh 'make O=out laptop_defconfig'
      }
    }

    stage('Kbuild') {
      steps {
        sh 'make -j16 O=out'
      }
    }

    stage('Install') {
      steps {
        sh '''sudo make O=out modules_install
sudo make O=out install'''
      }
    }

    stage('Build & install usbipd') {
      steps {
        sh '''cd tools/usb/usbip
sudo ./autogen.sh
sudo ./configure
sudo make install -j16
sudo cp libsrc/.libs/libusbip.so.0 /lib/libusbip.so.0
sudo apt-get install hwdata'''
      }
    }

  }
}