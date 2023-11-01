# Celery





```

# 启动 Celery worker 
celery -A tasks worker --loglevel=info  -P eventlet


# 启动 Celery Beat 守护进程：
celery -A tasks beat --loglevel=info

# 启动 FLower 监控
celery -A tasks flower --port=5555 -P eventlet


celery -A job.celery.celery_job_pv worker --loglevel=info -P eventlet
celery -A job.celery.celery_job_pv beat --loglevel=info
celery -A job.celery.celery_job_pv flower --port=5555 -P eventlet
```

