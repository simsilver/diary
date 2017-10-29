Title: 导出git库中两次修改之间所有改动的文件 
Date: 2015-12-29 10:48
Tags: git, patch
Category: 备忘

~~~
git archive -o update.zip commit2 $(git diff --name-only commit1 commit2)
~~~

[参考链接](https://ruby-china.org/topics/5312)
