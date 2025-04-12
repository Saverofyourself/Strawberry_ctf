性能:

- 容器个人化 & 参赛者flag转优化
- 单题目多容器部署

## Start

作者系统：Linux kali 6.6.15-amd64 #1 SMP PREEMPT_DYNAMIC Kali 6.6.15-2kali1 (2024-05-17) x86_64 GNU/Linux

Install requirements:

```bash
apt install nginx docker-compose tmux
apt install python3 python3-dotenv python3-flask python3-psycopg-pool python3-passlib python3-psutil python3-gevent python3-rich python3-pycryptodome gunicorn
```

Run PostgreSQL, generate config file and admin password:

```
sysctl -w fs.aio-max-nr=1048576

cd Blueberry-CTF
python3 platform-init.py

# ...
# Creating pgsql_pgadmin_1 ... done
# Creating pgsql_adminer_1 ... done
# Creating pgsql_db_1      ... done
# DB init ok. sleep 10s.
# Erase database...
# Init database...
# admin user: lilac
# password: Rd********KSw
```

Set up nginx:
```bash
cp conf/nginx_site.conf /etc/nginx/sites-available/default
nginx -t
# nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
# nginx: configuration file /etc/nginx/nginx.conf test is successful

nginx -s reload
```

Start Blueberry:
```bash
# use tmux!
cd web

gunicorn app:app
# [2024-01-29 18:00:50 +0800] [376930] [INFO] Starting gunicorn 20.1.0
# [2024-01-29 18:00:50 +0800] [376930] [INFO] Listening at: http://127.0.0.1:11451 (376930)
# [2024-01-29 18:00:50 +0800] [376930] [INFO] Using worker: gevent
# [2024-01-29 18:00:50 +0800] [376931] [INFO] Booting worker with pid: 376931
```

Now Blueberry platform is avaliable on port 80.

Build the demo challenge `runma`:
```
cd problems/runma
docker-compose build
```

Start the container manager:
```bash
# use tmux!

python3 backend.py
```

All done, enjoy your contest.
