#!/bin/bash

print_help () {
    echo "example: [program] -d [username] 192.168.1.1"
    echo "         [program] -u [username] 192.168.1.1 [file]"
}

monit_text () {
    local version="v1.0"
    printf "\nccd - Cisco configuration downloader $version\n\n"
}

date=`date +%Y-%m-%d_%H-%M-%S`
download_file="config_$date.txt"

scp_type="$1"
scp_user="$2"
scp_host="$3"

scp_file_destination="$PWD/$4"
scp_file_name="${scp_file_destination##*/}"

cipher="-c aes256-cbc"
algorithms="-oKexAlgorithms=+diffie-hellman-group1-sha1"

key="$scp_type"

case $key in
    -d|--download)
        monit_text
        if [ -z $scp_host ] ; then
            if [ -z $scp_user ] ; then
                echo "User is empty"
            else
                echo "User: $scp_user"
            fi
            echo "Host: $scp_host"
            exit 1
        else
            echo "User: $scp_user"
            echo "Host: $scp_host"
            echo "File: $download_file"
        fi
        mkdir -p "$scp_host"
        cd $scp_host
        scp $algorithms $cipher "$scp_user@$scp_host:running-config" "$download_file"
    ;;
    -u|--upload)
        monit_text
        if [ -z $scp_file_destination ] ; then
            if [ -z $scp_user ] ; then
                echo "User is empty"
            else
                echo "User: $scp_user"
            fi
            if [ -z $scp_host ] ; then
                echo "Host is empty"
            else
                echo "Host: $scp_host"
            fi
            echo "File is empty"
            exit 1
        else
            echo "User: $scp_user"
            echo "Host: $scp_host"
            echo "File: $scp_file_name"
        fi
        scp $algorithms $cipher "$scp_file_destination" "$scp_user"@"$scp_host:flash:/$scp_file_name"
    ;;
    -h|--help)
        print_help
    ;;
    *)
    echo "Bad argument"
    print_help
esac

