# info

here will record me develop deep learning model for mood analysis

## spec
- dell G15
- i5 12500H
- RTX 3050 Ti

- Anaconda for virtual environment
- Visual studio code

## develop process

1. install tensorflow
```python
conda install tensorflow
```

2.model.fit()方法來訓練模型



## error 
1. OMP: Error #15: Initializing libiomp5md.dll, but found libiomp5 already initialized.

add this code at first place
```
import os
os.environ['KMP_DUPLICATE_LIB_OK'] = 'True'
```