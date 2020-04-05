# onlyoffice-chinese-fonts
旨在解决onlyoffice-document-server中的中文字体名称显示问题<br>
将字体文件单独下载到fonts目录。在fonts目录的上一目录中执行如下命令：
```
sudo docker cp ./fonts/ 容器名:/usr/share/fonts/truetype/
```
进入容器，会发现在truetype下又多了一个fonts目录。执行
```
fc-cache -f -v
documentserver-generate-allfonts.sh
```
刷新浏览器缓存，重新进入onlyoffice编辑器，在字体列表中应该能看到字体的中文名了。
