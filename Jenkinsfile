pipeline {
    agent none
stages {
        stage('backup_audio_recordings_lenovo') {
            agent {
                label 'WIN-01-LENOVO'
            }
            steps {
                pwsh"""
                Get-PSDrive C
                "{0:N2} GB" -f ((Get-ChildItem \$env:REAPER | Measure-Object Length -Sum).sum / 1Gb)
                Set-Location \$env:REAPER
                # For full replace, uncomment:
                # bash -c "rm -rf ~/music/*"
                # bash -c "rsync --archive --progress * ~/music"
                # bash -c "df -h | head -n1 ; df -h | grep /mnt/c"
                # For full replace, uncomment:
                # aws s3 rm s3://music-bucket-reaper --recursive
                aws s3 sync . s3://music-bucket-reaper
                aws s3 sync s3://music-bucket-reaper .
                """
            }
        }
        stage('backup_audio_recordings_fedora') {
            agent {
                label 'WIN-01-LENOVO'
            }
            steps {
                pwsh"""
                Get-PSDrive C
                "{0:N2} GB" -f ((Get-ChildItem \$env:REAPER | Measure-Object Length -Sum).sum / 1Gb)
                Set-Location \$env:REAPER
                # For full replace, uncomment:
                # bash -c "ssh root@UNIX-02-FEDORA 'rm -rf /music/*'"
                bash -c "rsync --archive --progress --whole-file * root@UNIX-02-FEDORA:/music/"
                bash -c "ssh root@UNIX-02-FEDORA 'df -h | head -n1 ; df -h | grep /dev/mapper/fedora_fedora-root'"
                """
            }
        }
    stage('backup_audio_recordings_debian') {
            agent {
                label 'WIN-01-LENOVO'
            }
            steps {
                pwsh"""
                Get-PSDrive C
                "{0:N2} GB" -f ((Get-ChildItem \$env:REAPER | Measure-Object Length -Sum).sum / 1Gb)
                Set-Location \$env:REAPER
                # For full replace, uncomment:
                # bash -c "ssh root@UNIX-01-DEBIAN 'rm -rf /music/*'"
                bash -c "rsync -avz -e 'ssh -p 22' --archive --progress * root@UNIX-01-DEBIAN:/music/"
                bash -c "ssh root@UNIX-01-DEBIAN 'df -h | head -n1 ; df -h | grep /dev/mmcblk1p2'"
                """
            }
        }
        stage('backup_audio_recordings_asus') {
            agent {
                label 'WIN-02-ASUS'
            }
            steps {
                pwsh"""
                Get-PSDrive C
                "{0:N2} GB" -f ((Get-ChildItem \$env:REAPER | Measure-Object Length -Sum).sum / 1Gb)
                Set-Location \$env:REAPER
                aws s3 sync . s3://music-bucket-reaper
                # For full replace, uncomment:
                # bash -c "rm -rf ~/music/*"
                bash -c "rsync --archive --progress * ~/music"
                bash -c "df -h | head -n1 ; df -h | grep /mnt/c"
                # For full replace, uncomment:
                # aws s3 rm s3://music-bucket-reaper --recursive
                aws s3 sync s3://music-bucket-reaper .
                """
            }
        }
    }
}
