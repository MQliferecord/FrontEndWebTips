# git操作问题汇总

去写这个文档的原因，是因为我今天在按照往常的方式使用`git push -u origin master`命令的时候，出现了一个报错，显示本地文件和远程仓库的内容不符合。

起因是之前某天我在提交文件后，在github看效果的时候发现了一个错字就顺手在网页上修改了，导致了这个错误，在网上查了一些办法，最后解决的方案是，使用强覆盖方式合并本地内容和远程仓库。

```
git push -f
```
参考文档：
!(Git一个关于Push失败的两种解决方案)[https://www.jianshu.com/p/ea6ec80ad5f2]

> 线上代码和本地代码分支不同时，应该如何操作git？

例如：线上代码是分支branchA;本地修复bug代码是branchB。上线时应该如和操作？

- 可以使用merge或者是rebase。

有两种处理办法：

第一种：

```
//切换到主分支
git checkout branchA
//合并副分支
git merge branchB
//上线
git push origin branchA
```

第二种：

```
//切换到副分支
git checkout branchB
//更新主分支内容到副分支
git rebase branchA
//切换到主分支
git checkout branchA
//合并副分支
git merge branchB
//上线
git push origin branchA
```