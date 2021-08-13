## Perceptron 
![image.png](attachment:image.png)
y ={
0 (w1​x1​ + w2​x2​ < 0)
1 (w1​x1​ + w2​x2​ ≥ 0)
입력층과 출력층으로만 구성된다면 단층신경망 또는 단층퍼셉트론이라고 하고
비선형 데이터이기 때문에 단층신경망으로 학습이 불가능하다면 입력층과 출력층 사이에 하나 이상의 은닉층을 
두어 데이터를 학습하는 것을 다층신경망 또는 다층 퍼셉트론이라고 합니다. 


```python
import tensorflow as tf
import numpy as np
```


```python
class Perceptron:
    def __init__(self, w, b):
        self.w = tf.Variable(w, dtype=tf.float32)
        self.b = tf.Variable(b, dtype=tf.float32)
        
    def __call__(self, x):
            return tf.sign(tf.reduce_sum(self.w*x) + self.b)
```


```python
def v(*args):
    return np.array(args)
```


```python
w = v(1,1)
b = 0.5

perceptron = Perceptron(w, b)
```


```python
p1 = perceptron(v(1,1))
p2 = perceptron(v(-1,1))
p3 = perceptron(v(-1,-1))
p4 = perceptron(v(1,-1))

print(p2.numpy(), p1.numpy())
print(p3.numpy(), p4.numpy())
```

    1.0 1.0
    -1.0 1.0
    
