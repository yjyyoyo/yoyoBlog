# 一.Git 配置的代理端口不一致
![[Pasted image 20261103072413.png|296]]

问题原因是 Git 配置的代理端口不一致：

- `https.proxy` 使用 `127.0.0.1:7890`
- `http.proxy` 错误地使用了 `127.0.0.1:7892`
- 推送 GitHub 时通过错误代理端口连接失败

解决方法是清除旧代理配置，并统一设置为可用的 `127.0.0.1:7890`：

```bash
git config --global --unset-all http.proxy
git config --global --unset-all https.proxy

git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
```

之后使用 `curl` 验证代理能够访问 GitHub，确认连接正常后重新执行 `git push`，推送成功。