# Matplotlib Tips
## Exporting SVG files with Matplotlib
调整配置：让Matplotlib输出矢量图SVG文件[^1]
```c
import matplotlib
import matplotlib.pyplot as plt
%matplotlib inline
%config InlineBackend.figure_format = 'svg'
```
将生成的矢量图保存为`.pdf`或者`.eps`插入LaTex
```c
plt.savefig('tmp.eps', bbox_inches = 'tight')
plt.show()
```

---

## References
[^1]:https://www.zhihu.com/question/59392251/answer/403124614