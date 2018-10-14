以下 `clone` 指令都在統一的 workspace 目錄下執行。

## Blog repo
複製開發用的 branch
```commandline
git clone -b dev https://github.com/leemengtaiwan/leemengtaiwan.github.io.git
```

## Pelican 插件 / 主題
統一使用官方 plugins，ipynb plugin 及 theme 則使用自己開發的 repo
```commandline
git clone https://github.com/leemengtaiwan/pelican-jupyter-notebook.git
git clone --recursive https://github.com/getpelican/pelican-plugins
cd pelican-plugins/pelican-ipynb/
git remote add leemengtaiwan https://github.com/leemengtaiwan/pelican-ipynb.git
git fetch leemengtaiwan
git checkout dev
```

pelican-toc
```commandline
cd pelican-plugins/pelican-toc/
git remote add leemengtaiwan https://github.com/leemengtaiwan/pelican-toc.git
git fetch leemengtaiwan
git checkout dev
```

## 第一次修改插件
首先先手動 fork 在 `pelican-plugins` 裡頭想要修改的 submodule，再 clone 下來修改
```commandline
git remote add leemengtaiwan https://github.com/leemengtaiwan/pelican-toc.git
git fetch leemengtaiwan
git checkout leemengtaiwan/master
...
git checkout -b dev
git commit -m "Finish!"
git push --set-upstream leemengtaiwan dev
```

## 建立開發基本環境

```commandline
conda create -n blog python=3
source activate blog
pip install -r init_requirements.txt
```

## 修改插件 / 主題設定
- 修改 `pelicanconf.py`
    - `THEME` 指到 `WORKSPACE/pelican-jupyter-notebook/themes/Hola10`
    - `PLUGIN_PATHS` 指到 `WORKSPACE/pelican-plugins`


## Blog 開發
- Shell 快捷鍵參考 [Gist](https://gist.github.com/leemengtaiwan/0fb24bdd4d33fefad39d0c718413880f)
- 啟動對應的開發環境，例： `source activate blog`

在 dev branch 重新建立文章
```commandline
cd leemengtaiwan.github.io
python copy_static_files.py; pelican content
```

只更新特定文章
```commandline
pelican content -r 
```



開啟 HTTP 伺服器確認
```commandline
cd leemengtaiwan.github.io/output
python -m pelican.server
```

更新 dev 分支
```commandline
git commit -m "Update A post"
```

發布文章到 master branch
```commandline
cd leemengtaiwan.github.io/
pelican content -s publishconf.py
python add_templates.py
ghp-import output -b master -m "Commit Message Here"
git push origin master
```

## 文章內容

Meta Cell template

```markdown
- author: Lee Meng
- date: 2018-07-01 12:00
- title: New Post
- slug: just-a-test-url
- tags: 機器學習, R
- description: This is a description
- summary: This is a summary
- image: andy-kelly-402111-unsplash.jpg
- image_credit_url: https://www.google.com
- enable_notebook_download: false
- status: draft

```

文摘用 template
```markdown
!article
- Data visualisation, from 1987 to today
- https://medium.economist.com/data-visualisation-from-1987-to-today-65d0609c6017
- leroy-stencil-set.jpg
```

單一圖片
```text
<img src="{filename}images/gapminder/chuttersnap-176806-unsplash.jpg"/>
<br>
```

圖片＋下標
```text
<center>
    <img src="{filename}images/gapminder/official-page.jpg" style=""/>
</center>
<center>
    GapMinder 釋出的<a href="https://www.gapminder.org/tools/#" target="_blank">泡泡圖</a>截圖。在右邊的清單搜尋「 Taiwan 」不會有結果
    <br/>
    <br/>
</center>
```

video
```text
<video loop muted autoplay playsinline poster="{filename}images/gapminder/decline-of-female-fertility.jpg">
  <source src="{filename}images/gapminder/decline-of-female-fertility.mp4" type="video/mp4">
    您的瀏覽器不支援影片標籤，請留言通知我：Ｓ
</video>
<br>
```


