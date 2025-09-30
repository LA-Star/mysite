# Linux安装字体

## 字体资源准备

- 方案一：字体可以自行从 windows 系统下进入 C:\windows\Fonts 文件夹，挑选常用的中文字体拷贝到 U 盘。
- 方案二：为方便需要的人，博主已从 windows 系统中提取了常用中文字体并打包,直链地址：https://res.frytea.com/Others/Fonts/win_cn_fonts.zip

## 拷贝字体到linux字体目录

移动字体库到 linux 系统下的字体库文件夹 /usr/share/fonts/ 下

## 刷新字体目录

```shell
#让 linux 系统识别新的中文字体
sudo fc-cache -fv
```

## 查看是否安装成功

如需确认新的中文字体库是否已经安装，可在终端中输入：

```shell
$ fc-list :lang=zh-cn | sort
...
###
输出类型下面内容
/usr/share/fonts/win_font/Dengb.ttf: 等线,DengXian:style=Bold
/usr/share/fonts/win_font/Dengl.ttf: 等线,DengXian,DengXian Light,等线 Light:style=Light,Regular
/usr/share/fonts/win_font/Deng.ttf: 等线,DengXian:style=Regular
/usr/share/fonts/win_font/FZLTCXHJW.ttf: 方正兰亭超细黑简体,FZLanTingHeiS\-UL\-GB:style=Regular
/usr/share/fonts/win_font/FZSTK.TTF: 方正舒体,FZShuTi:style=Regular
/usr/share/fonts/win_font/FZYTK.TTF: 方正姚体,FZYaoTi:style=Regular
/usr/share/fonts/win_font/msyhbd.ttc: 微软雅黑,Microsoft YaHei:style=Bold,Negreta,tučné,fed,Fett,Έντονα,Negrita,Lihavoitu,Gras,Félkövér,Grassetto,Vet,Halvfet,Pogrubiony,Negrito,Полужирный,Fet,Kalın,Krepko,Lodia
/usr/share/fonts/win_font/msyhbd.ttc: Microsoft YaHei UI:style=Bold,Negreta,tučné,fed,Fett,Έντονα,Negrita,Lihavoitu,Gras,Félkövér,Grassetto,Vet,Halvfet,Pogrubiony,Negrito,Полужирный,Fet,Kalın,Krepko,Lodia
/usr/share/fonts/win_font/msyhl.ttc: 微软雅黑,Microsoft YaHei,Microsoft YaHei Light,微软雅黑 Light:style=Light,Regular
/usr/share/fonts/win_font/msyhl.ttc: Microsoft YaHei UI,Microsoft YaHei UI Light:style=Light,Regular
/usr/share/fonts/win_font/msyh.ttc: 微软雅黑,Microsoft YaHei:style=Regular,Normal,obyčejné,Standard,Κανονικά,Normaali,Normál,Normale,Standaard,Normalny,Обычный,Normálne,Navadno,Arrunta
/usr/share/fonts/win_font/msyh.ttc: Microsoft YaHei UI:style=Regular,Normal,obyčejné,Standard,Κανονικά,Normaali,Normál,Normale,Standaard,Normalny,Обычный,Normálne,Navadno,Arrunta
/usr/share/fonts/win_font/simfang.ttf: 仿宋,FangSong:style=Regular,Normal,obyčejné,Standard,Κανονικά,Normaali,Normál,Normale,Standaard,Normalny,Обычный,Normálne,Navadno,Arrunta
/usr/share/fonts/win_font/simhei.ttf: 黑体,SimHei:style=Regular,Normal,obyčejné,Standard,Κανονικά,Normaali,Normál,Normale,Standaard,Normalny,Обычный,Normálne,Navadno,Arrunta
/usr/share/fonts/win_font/simkai.ttf: 楷体,KaiTi:style=Regular,Normal,obyčejné,Standard,Κανονικά,Normaali,Normál,Normale,Standaard,Normalny,Обычный,Normálne,Navadno,Arrunta
/usr/share/fonts/win_font/SIMLI.TTF: 隶书,LiSu:style=Regular
/usr/share/fonts/win_font/simsun (2).ttc: 宋体,SimSun:style=常规,Regular
/usr/share/fonts/win_font/simsun (2).ttc: 新宋体,NSimSun:style=常规,Regular
/usr/share/fonts/win_font/simsun.ttc: 宋体,SimSun:style=常规,Regular
/usr/share/fonts/win_font/simsun.ttc: 新宋体,NSimSun:style=常规,Regular
/usr/share/fonts/win_font/SIMYOU.TTF: 幼圆,YouYuan:style=Regular
/usr/share/fonts/win_font/STCAIYUN.TTF: 华文彩云,STCaiyun:style=Regular
/usr/share/fonts/win_font/STFANGSO.TTF: 华文仿宋,STFangsong:style=Regular
/usr/share/fonts/win_font/STHUPO.TTF: 华文琥珀,STHupo:style=Regular
/usr/share/fonts/win_font/STKAITI.TTF: 华文楷体,STKaiti:style=Regular
/usr/share/fonts/win_font/STLITI.TTF: 华文隶书,STLiti:style=Regular
/usr/share/fonts/win_font/STSONG.TTF: 华文宋体,STSong:style=Regular
/usr/share/fonts/win_font/STXIHEI.TTF: 华文细黑,STXihei:style=Regular
/usr/share/fonts/win_font/STXINGKA.TTF: 华文行楷,STXingkai:style=Regular
/usr/share/fonts/win_font/STXINWEI.TTF: 华文新魏,STXinwei:style=Regular
/usr/share/fonts/win_font/STZHONGS.TTF: 华文中宋,STZhongsong:style=Regular
```

