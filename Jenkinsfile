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
      parallel {
        stage('Kbuild') {
          steps {
            sh 'make -j16 O=out'
          }
        }

        stage('Build usbipd') {
          steps {
            sh '''sudo ./tools/usb/usbip/autogen.sh
sudo ./tools/usb/usbip/configure
'''
          }
        }

      }
    }

    stage('Install') {
      parallel {
        stage('Install') {
          steps {
            sh '''sudo make O=out modules_install
sudo make O=out install'''
          }
        }

        stage('Install usbipd') {
          steps {
            sh '''sudo make -C tools/usb/usbip install -j16
sudo cp tools/usb/usbip/libsrc/.libs/libusbip.so.0 /lib/libusbip.so.0'''
          }
        }

      }
    }

  }
}