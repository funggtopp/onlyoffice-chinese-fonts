# onlyoffice-chinese-fonts
旨在解决在docker中运行的onlyoffice-document-server中文字体名称显示问题<br>
<br><br>
## for onlyoffice 6.3.X
字体完美。已全部上传完毕。<br>
6.X的解决与5.X的解决还是有些差别的。<br>
为保证完美效果，请将6.X版本的字体全部下载并导入容器。<br>
单字体使用也是可以的！！！<br>
先进入docker容器。<br>

```
sudo docker exec -it oo6 /bin/bash
#进入容器后执行
cd /usr/share/fonts/
rm -rf *
cd /var/www/onlyoffice/documentserver/core-fonts/
rm -rf *
```
将原来字体删除。<br>
再将字体全部下载到容器的`/usr/share/fonts`目录下。<br>
在容器内执行<br>
`
 /usr/bin/documentserver-generate-allfonts.sh
`
<br>
将浏览器缓存清空，即可正常使用。<br>
最终效果图：<br>
<img src=https://github.com/funggtopp/onlyoffice-chinese-fonts/raw/master/onlyoffice_chinesefonts.jpg width=60%></img>

<br><br>
## mini_fonts精简版本
原版字体文件略大，平均每个在15M左右。国标字体略小。<br>
宋黑仿楷四大常用字体都做了相应压缩，包含了常用汉字及字符。<br>
onlyoffice缺省字体Calibri使用等线做了伪装。<br>
如果有部分字符不能显示或方框，切换成雅黑，雅黑字没有精简。<br>
经实验可以在一定程度上减少编辑网页首次加载时间。<br>
如果带宽足够就没有必要了。
<br><br>
## for onlyoffice 5.X
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

*** 注意 *** ：建议不要删除容器里原来fonts目录里的字体，可能会出现方框问题。
<br><br><br>
## 字号问题
中文还是习惯小初、小四之类的。<br>
将<br>
/var/www/onlyoffice/documentserver/web-apps/apps/documenteditor/main/<br>
目录下的app.js做修改即可。<br>
注意：一共有6个app.js文件，我只修改了文档编辑器的电脑版本。其他如电子表格、幻灯片及移动版本没有修改。因为基本用不到。
<br>
app.js需要从容器拷贝出来后修改，直接修改未必会成功。<br>
在app.js中查找`{value:8,displayValue:"8"}`字符串，将相应字符串替换为:<br>
```
{value:42,displayValue:"初号"},{value:36,displayValue:"小初"},{value:26,displayValue:"一号"},{value:24,displayValue:"小一"},{value:22,displayValue:"二号"},{value:18,displayValue:"小二"},{value:16,displayValue:"三号"},{value:15,displayValue:"小三"},{value:14,displayValue:"四号"},{value:12,displayValue:"小四"},{value:10.5,displayValue:"五号"},{value:9,displayValue:"小五"},{value:7.5,displayValue:"六号"},{value:6.5,displayValue:"小六"},{value:5.5,displayValue:"七号"},{value:5,displayValue:"八号"},
```
<br>
建议保留后面的value:48等值。
