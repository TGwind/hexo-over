---
title:  部署Hexo到Linux服务器
cove: http://myblog.over2022.top/image-20231011175222440.png
tags:
-  静态网站
-  Hexo
categories:
- 服务器运维
---

1. **在本地生成静态网页：** 在本地的Hexo博客目录中，运行以下命令生成静态网页：

   ```
   hexo clean
   hexo generate
   ```

   这将生成静态网页文件，并存储在Hexo博客目录的 `public` 文件夹中。

2. **连接到服务器：** 使用SSH工具（如OpenSSH、PuTTY等），通过服务器的IP地址和登录凭据（用户名和密码或SSH密钥）连接到服务器。

3. **创建目标文件夹：** 在服务器上选择一个目标文件夹，用于存放你的Hexo博客。你可以使用命令创建目标文件夹：

   ```
   mkdir <target_folder>
   ```

   替换 `<target_folder>` 为你想要创建的目标文件夹的路径。

4. **将本地文件上传到服务器：** 在本地的Hexo博客目录中，使用SCP命令将生成的静态网页文件上传到服务器的目标文件夹中：

   ```
   scp -r public/* <username>@<server_ip>:<target_folder>
   ```

   替换 `<username>` 为服务器的用户名，`<server_ip>` 为服务器的IP地址，`<target_folder>` 为目标文件夹的路径。

   这将递归地复制本地的静态网页文件到服务器上的目标文件夹。

5. **配置Web服务器：** 在服务器上配置一个Web服务器（如Nginx、Apache等），并将其配置为提供Hexo博客的静态网页。具体的配置方法可以参考相应的Web服务器文档。

6. **访问博客：** 完成文件上传后，你可以通过浏览器访问服务器的IP地址或域名来查看部署的Hexo博客。

### 配置Ngnix

如果你想配置Nginx作为你的Web服务器，以下是一些基本的步骤：

1. **安装Nginx：** 在服务器上运行以下命令来安装Nginx：

   ```
   sudo apt-get update
   sudo apt-get install nginx
   ```

   这将更新软件包列表并安装Nginx。

2. **配置网站文件夹：** 默认情况下，Nginx的网站文件夹位于`/var/www/html`。你可以根据需要选择不同的目录和文件夹名称。

   宝塔面板的Nginx网站文件夹位于 /www/server/nginx/html

   ```
   sudo mkdir -p /var/www/html
   ```

   这将在`/var/www/html`目录下创建一个用于存放网站文件的文件夹。

3. **将网站文件复制到服务器上：** 将你的静态网站文件复制到服务器上的目标文件夹中。你可以使用SCP命令或其他文件传输工具将文件从本地复制到服务器上。

   ```
   scp -r /path/to/your/website/* <username>@<server_ip>:/var/www/html
   ```

   替换`/path/to/your/website`为你本地静态网站文件的路径，`<username>`为服务器的用户名，`<server_ip>`为服务器的IP地址。

4. **配置Nginx：** 打开Nginx的配置文件`/etc/nginx/nginx.conf`或`/etc/nginx/sites-available/default`，进行相应的配置。

   - 指定网站的根目录：找到`root`指令，并将其设置为你的网站文件夹的路径，例如：

     ```
     root /var/www/html;
     ```

   - 配置域名和端口：如果你有自定义的域名，可以在配置文件中设置域名，并配置监听的端口。以下是一个示例：

     ```
     server {
         listen 80;
         server_name example.com;
         root /var/www/html;
     }
     ```

   这将使Nginx监听80端口，并将请求转发到指定的网站根目录。

   使用宝塔面板配置Nginx：

   ![image-20231011175222440](http://myblog.over2022.top/image-20231011175222440.png) 

   ```
   server {
       listen 80;
       server_name hexo.over2022.top;
       root /www/server/nginx/html/public;
   }
   ```

   

5. **重启Nginx：** 在完成配置后，重启Nginx以使配置生效。

   ```
   sudo service nginx restart
   ```

   这将重新启动Nginx服务，并加载新的配置，之后就可以通过域名访问自己的博客啦~
