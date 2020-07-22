# How to contribute

This document describes contribution process for Tahoma User Manual.

## Required Softwares on Windows

- Install [Git for Windows](https://git-for-windows.github.io/)

  Git Bash will be installed accordingly.

- Install Python

  Command Prompt will work if Python is added to PATH.

- (Optional) Install Anaconda
  
  Confirmed working with [Anaconda](https://www.anaconda.com/download/) distribution Python 2.7 version. Python 3.6 version should work, I guess.
  
  Anaconda Prompt will be installed accordingly. Command Prompt will work if the Python is added to PATH on installation.

## Preparation (to be done once for the first time)

1. `fork` Tahoma to your GitHub account from `tahoma/tahoma_docs`.

   - click the `fork` button at the top-right corner of https://github.com/tahoma/tahoma_docs

1. `clone` the repository. (w/ Git Bash)

   - Launch Git Bash.
   
   - Move to the folder you would like to make your local repository.
   
     `$ cd C:path/to/the/parent/folder`
     
   - Clone
   
     `$ git clone https://github.com/your_github_account_name/tahoma_docs.git`
     
     The folder `tahoma_docs` will be created under the current folder.
     
   - Move into tahoma local repository
   
     `$ cd tahoma_docs`
     
   - Set the `upstream` , in order to synchronize your repository with the latest version.
   
     `$ git remote add upstream https://github.com/tahoma/tahoma_docs.git`

1. Prepare Python and Sphinx (w/ Command Prompt)

   - Install Sphinx
   
     `$ pip install sphinx sphinx-autobuild`
     
   - Install `sphinx_rtd_theme` theme
   
     `$ pip install sphinx_rtd_theme`

   - Install internationalization(i18n) package
   
     `$ pip install sphinx-intl`
     
1. Try building the HTML (w/ Command Prompt)

   - Move to `tahoma_docs` folder using `cd` command

   - Make HTML
   
     `$ make html`
     
     The result HTML files will be generated under `build/html/`

## Workflow (to be done each time you modify the code)

First , launch Git Bash and move to `tahoma_docs` folder using `cd` command

1. Synchronize the local repository with the latest code from `upstream` (w/ Git Bash) 

    - Change the current branch to `master`
    
      `$ git checkout master`
      
    - Obtain the latest code
    
      `$ git pull upstream master`
      
    - Synchronize your `origin` with the local repository
    
      `$ git push origin master`

1. Checkout the branch and modify the code (w/ Git Bash)
    
    - `$ git checkout -b your-branch-name`
    
       `your-branch-name` is a name of your modifications, say, `typo_fix`, `how_to_install` or something.
    
    - Fix codes (see the [Editing reStructuredText](#editing-restructuredtext) below).
        
    - `$ git status` can show a list of files you have modified
    
    - `$ gitk` can show details of modification

    - `$ make html` to check the generated HTML locally

1. Commit (w/ Git Bash)

    - Do one of the followings:
    
      - `$ git status` to see the files you have modified.
      
         `$ git add path/to/file` for all the files you would like to register in the commit
         
         `$ git commit -m "some_good_commit_message"`
         
      - Use `-a` option to commit all the modified files
      
         `$ git commit -a -m "some_good_commit_message"`
    
1. Fix conflict (w/ Git Bash)

    - In order to make sure your modification will not conflict with the current upstream, `pull` the latest changes form the `master` branch of the upstream.
      
      `git pull upstream master` or `git pull --rebase upstream master`
      
    - If there is conflicts, message will appear like;
    
      `CONFLICT (content): Merge conflict in path/to/conflicted/file.rst`
      
    - Use `$ git status` to list conflicted files
    
    - Open the conflicted file and resolve the coflicted part (appears as below) manually
      
      ```
      path/to/conflicted/file.rst
      
      <<<<<<< HEAD
      CHANGES_IN_YOUR_COMMIT
      =======
      CHANGES_IN_UPSTREAM
      >>>>>>> [commit id]
      ```
    
    - `$ git add path/to/conflicted/file.rst` for all conflicted files
    
    - `$ git commit`
         
1. Push your branch and make a pull request (w/ Git Bash)

    - `$ git push origin your-branch-name`
    
    - Make a pull request
      
      Access https://github.com/tahoma/tahoma_docs and click Pull Request Button.

Also you can refer to the [git documentation](https://git-scm.com/book/en/v2/GitHub-Contributing-to-a-Project).

## Editing reStructuredText

Please see [Sphinx/reStructuredText documentation](http://www.sphinx-doc.org/en/stable/rest.html) for details.

### Local rules (for the moment)

- In order to avoid getting mess, please put image files in the folder with the same name as .rst file. 
  For example, if you add an image to `source/installing_tahoma.rst`, 
  then make a folder named `installing_tahoma` under `source/_static` and store the image file in it
  (i.e. `source/_static/installing_tahoma/some_image.png` ).

## Internationalization (i18n) of the Manual

- On Anaconda Prompt, move to `tahoma_docs` folder using `cd` command

- Generate the message catalogue (.pot)

  `$ make gettext`

  .pot files will be generated in `build/gettext` .

- Copy .pot to your language's locale folder

  `$ sphinx-intl update -p build/gettext -l ja`

  Please replace `ja` by the language you would like to translate. See [the sphinx manual](http://www.sphinx-doc.org/en/stable/config.html#confval-language) for supported languages.
  
  Then .po files will be generated in `source/locale/ja/LC_MESSAGES`.

- To update .po to the latest version, do the same commands again;

  ```
  $ make gettext
  $ sphinx-intl update -p build/gettext -l ja
  ```

- Translate the document in .po files

  In .po files, there are list of messages like:

  ```
  #: ../../source/index.rst:7
  msgid "Tahoma User Manual"
  msgstr ""
  ```

  Input translation into `msgstr` like as follows:

  ```
  #: ../../source/index.rst:7
  msgid "Tahoma User Manual"
  msgstr "Tahoma ユーザマニュアル"
  ```

  Note: Make sure to maintain the syntax of reStructuredText in your translated text.

- Generate the translated-version HTML locally

  `$ make SPHINXOPTS="-D language='ja'" html`
  or
  ```
  $ set SPHINXOPTS=-D language=ja
  $ make.bat html
  ```
  
- NOTE : To include the translation to the User Manual, some settings in Read the Docs is needed. Please let me (@shun-iwasawa) know when you firstly merge your language translation.
