



# 初始化

```bash
git config --global user.name "koobro"
git config --global user.email "koobro@qq.com"
```



# 克隆项目

```bash
git clone https://gitee.com/koobro/koobro-project.git
```



# 推送失败

fatal: unable to access 'https://github.com/koobro100/PythonTutorial.git/': Failed to connect to github.com port 443 after 21074 ms: Couldn't connect to server

```bash
//取消http代理
git config --global --unset http.proxy
//取消https代理 
git config --global --unset https.proxy
```

