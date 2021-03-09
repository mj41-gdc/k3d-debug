# Debug K3d restarts

    # setup
    mkdir -p ~/devel/k3d
    cd ~/devel/k3d
    pwd
    ls -al

    # console 1
    # check logs and send them to github if all seems ok
    ls -als log*.txt
    echo '```' ; head -n 10 log*.txt ; echo '```'
    # cleanup previous run
    k3d cluster delete default
    rm ~/devel/k3d/log-*.txt
    ls -al
    cat README.md

    # console 2
    echo "#Start: `date --rfc-3339=ns`" > log-docker-events.txt ; docker events | tee -a log-docker-events.txt #c2

    # console 3
    echo "#Start: `date --rfc-3339=ns`" > log-start-k3d.txt ; k3d cluster create default --image rancher/k3s:v1.20.4-k3s1 -v /dev/mapper:/dev/mapper -v /lib/modules:/lib/modules 2>&1 | tee -a log-start-k3d.txt ; echo "#End: `date --rfc-3339=ns`" >> log-start-k3d.txt #c3

    # console 4
    # a few times run these till you see the first restart
    echo "#Start_ps: `date --rfc-3339=ns`" | tee -a log-docker-ps.txt ; docker ps | tee -a log-docker-ps.txt #c4

    # console 5
    echo "#Start_logs: `date --rfc-3339=ns`" | tee -a log-docker-logs.txt ; docker logs --timestamps --details k3d-default-server-0 2>&1 | tee -a log-docker-logs.txt #c5

    # console 2
    # Press ctrl+c

    # check logs, repeat if needed
