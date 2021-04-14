# 本文作用：在ubnutu下使用梯子shadowfly

## 一、地址：
https://shadowfly.cloud <br>
购买了套餐后，可以在“下载/教程-系统”查看使用教程。

## 二、从window客户端下载账号配置文件

### 1、按照windows教程在window下进行配置

[shadowfly-Windows使用教程](https://shadowfly.cloud/user/tutorial?os=windows&client=cfw)

### 2、配置文件
window客户端配置好后，在安装包目录下找到两个文件
- `ClashRforWindows/ClashR for Windows/resources/static/files/profiles`，找到刚刚导入配置的.yml文件，复制到新的文件夹，并改名为`config.yaml`;
- `ClashRforWindows/ClashR for Windows/resources/static/files`,找到`Country.mmdb`文件，复制到上面的新文件夹
- 切换到ubuntu系统，将上面两个文件，复制到linux端下载的可执行文件的文件夹下
到这里，配置文件步骤完成

## 三、使用方法
 
### 1、对网络设置

<center class="half">
    <img src="https://pic4.zhimg.com/80/v2-cf4b324814adf3d7b043cf662272e290_720w.png" width="600"/>
</center>

注意，设置HTTP、HTTPS、Socks主机三项。<br>
打开系统设置，选择网络，点击网络代理右边的 ⚙ 按钮，选择手动，填写 HTTP 和 HTTPS 代理为 127.0.0.1:7890，填写 Socks 主机为 127.0.0.1:7891，即可启用系统代理。

### 2、启动
- 下载linux的shadowfly可执行文件
[shadowfly-linux](https://github.com/Dreamacro/clash/releases)
选择合适的版本下载下来，解压，得到一个可执行文件，改名为`clash`

- 打开终端

    $ mkdir clash

    $ cd clash
    
	将上面得到的clash可执行文件放入clash文件夹中
	
    $ ./clash -d .

        若提示权限不足，执行 `chmod +x clash` 
    
        再执行 ./clash -d .

        注意，在使用期间，这个命令不能关闭

    $ 打开浏览器，使用google测试

### 3、使用Proxy SwitchyOmega自动切换代理

[Proxy SwitchyOmega](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif?hl=zh-CN)

- 下载安装

    [SwitchyOmega下载安装](https://proxy-switchyomega.com/download/)

- 使用指南

    [使用指南](https://tmr.js.org/p/73acc153/)

- 使用SwitchyOmega自动切换代理后，系统设置->选择网络，就可设置为自动。
