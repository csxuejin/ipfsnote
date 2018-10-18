# supervisor 管理 IPFS

参考配置：

```
[program:ipfs]
command=/bin/bash -c "/usr/local/bin/ipfs daemon"
environment=HOME="/home/deploy",USER="deploy"
user=deploy
process_name=%(program_name)s ; process_name expr (default %(program_name)s)
numprocs=1                    ; number of processes copies to start (def 1)
startsecs=1                   ; number of secs prog must stay running (def. 1)
startretries=3                ; max # of serial start failures (default 3)
user=deploy                     ; setuid to this UNIX account to run the program
redirect_stderr=true          ; redirect proc stderr to stdout (default false)
stdout_logfile=/home/deploy/logs/ipfs.stdout.log
stdout_logfile_maxbytes=50MB  ; max # logfile bytes b4 rotation (default 50MB)
stdout_logfile_backups=10     ; # of stdout logfile backups (default 10)
stdout_capture_maxbytes=1MB   ; number of bytes in 'capturemode' (default 0)
stdout_events_enabled=false   ; emit events on stdout writes (default false)
stderr_logfile=/home/deploy/logs/ipfs.stderr.log
stderr_logfile_maxbytes=10MB  ; max # logfile bytes b4 rotation (default 50MB)
stderr_logfile_backups=10     ; # of stderr logfile backups (default 10)
stderr_capture_maxbytes=1MB   ; number of bytes in 'capturemode' (default 0)
stderr_events_enabled=false   ; emit events on stderr writes (default false)
```

这个[提问](https://github.com/ipfs/go-ipfs/issues/861)解释了为什么需要这样配置 command 和 environment.

