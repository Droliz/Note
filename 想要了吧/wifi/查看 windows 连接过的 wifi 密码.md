# 查看 windows 连接过的 wifi 密码

使用管理员运行 windows 的命令提示窗口


查看已连接过的 wifi 名
```sh
netsh wlan show profile
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220519142717.png)

将所有的 wifi 配置文件导出

```sh
netsh wlan export profile folder=FILE_PATH key=clear
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220519142834.png)

随后在文件中找到 `keyMaterial` 标签包裹的就是密码

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220519143040.png)