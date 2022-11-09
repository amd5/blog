## 解决supervisord报错Too many open files 修改Linux的open files值
```bash
临时修改
ulimit -n 102400
查看当前值
ulimit -n
修改supervisord.conf的
minfds=1024
```
