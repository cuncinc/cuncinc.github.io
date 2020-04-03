学习了一个批量操作文件的小脚本

```shell
#!/bin/bash

for val in *.md *.txt	#对当前文件夹下的所有.md和.txt文件
do
	#要实现的功能
	echo "Create by cuncinc" >> ${val};
done
exit 0
```

