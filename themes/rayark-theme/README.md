# Rayark Documentation Theme

Rayark Documentation Theme 是基於 [Hugo Learn Theme](https://github.com/matcornic/hugo-theme-learn) 修改的 [Hugo](https://gohugo.io/)，作為 Rayark 內部技術文件撰寫使用。
其中優化版排與視覺風格，利於技術文件閱讀，並使風格和 Rayark 其它網頁一至。範例請參考 [Deemo Documentation](http://deemo.pages.rayark.io/doc/)。


詳細的功能和使用方法和 Hugo Learn Theme 相同，請參考 Hugo Learn Theme 之[說明文件](https://learn.netlify.com/en/)。



## 安裝 Hugo

詳細方法，請參考 [Hugo 的說明](https://gohugo.io/getting-started/installing/)。
## Usage ( Method #1 )

1. Fork the repository: https://gitlab.rayark.com/frontend/rayark-doc-example
2. Rename forked **project name** and **repository**. See [link](https://docs.gitlab.com/ee/user/project/settings/)
3. Clone the forked repository
4. Change the field `title` in `config.toml`
5. Init Git Submodule and update the theme submodule to the latest version 
   `git submodule update --init --remote`
6. Commit and push changes.



## Usage ( Method #2 )

### 建立專案


1. `$ hugo new site <new_project>`
2. 至 Gitlab 建立專案，例如 https://gitlab.rayark.com/deemo/doc
3. `$ cd <new_project>`   
`<new_project> $ git init .`   
`<new_project> $ git remote add origin git@gitlab.rayark.com:deemo/doc.git`
4. 將 rayark-doc-theme 以 submodule 的方式加入 themes 目錄下   
`<new_project> $ git submodule add ../../frontend/rayark-doc-theme.git themes/rayark-doc-theme`
5. 編輯 `config.toml` 並加入下面的設定   

    ```toml
    # Change the default theme to be use when building the site with Hugo
    
    theme = "rayark-doc-theme"

    # For search functionnality
    [outputs]
    home = [ "HTML", "RSS", "JSON"]
    ```
6. 添加 chapter 首頁   
`<new_project> $ hugo new --kind chapter <chapter_name>/_index.md`
7. 添加一般頁面   
`<new_project> $ hugo new <chapter_name>/<page_name>.md`   
或   
`<new_project> $ hugo new <chapter_name>/<content_name>/<page_name>.md`
8. 啓動本地測試 server   
`<new_project> $ hugo serve`

### Gitlab CI

請參考 Deemo Doc 專案的 ci [設定檔](https://gitlab.rayark.com/deemo/doc/blob/master/.gitlab-ci.yml)。
