# CMD Commands

## 1. Kill the process on a certain port

```
netstat -nao | findstr "9000"
taskkill -pid 15568 -f
```