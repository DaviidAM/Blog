# Operaciones

## Operaciones aritméticas

```py
arr = np.arange(0,11)
# array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10])

arr + arr
# array([ 0,  2,  4,  6,  8, 10, 12, 14, 16, 18, 20])
arr * arr
# array([ 0,  1,  4,  9, 16, 25, 36, 49, 64, 81])
arr - arr
# array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0])
arr/arr
# Warning ya que en uno de los casos dividimos entre cero, entonces en ese caso el resultado es nan.
# RuntimeWarning: invalid value encountered in divide
# array([nan,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.])
1/arr
# Warning ya que en en uno de los casos obtenemos infinito
# RuntimeWarning: divide by zero encountered in true_divide
# array([       inf, 1.        , 0.5       , 0.33333333, 0.25      , 0.2       , 0.16666667, 0.14285714, 0.125     , 0.11111111])
arr**3
# array([  0,   1,   8,  27,  64, 125, 216, 343, 512, 729], dtype=int32)
```

## Métodos para operaciones

```py
np.sqrt(arr)
'''
array([0.        , 1.        , 1.41421356, 1.73205081, 2.        , 
2.23606798, 2.44948974, 2.64575131, 2.82842712, 3.        ])
'''
np.exp(arr)
'''
array([1.00000000e+00, 2.71828183e+00, 7.38905610e+00, 2.00855369e+01,
       5.45981500e+01, 1.48413159e+02, 4.03428793e+02, 1.09663316e+03,
       2.98095799e+03, 8.10308393e+03])
'''
np.sin(arr)
'''
array([ 0.        ,  0.84147098,  0.90929743,  0.14112001, -0.7568025 ,
       -0.95892427, -0.2794155 ,  0.6569866 ,  0.98935825,  0.41211849])
'''
np.log(arr)
'''
array([      -inf, 0.        , 0.69314718, 1.09861229, 1.38629436,
       1.60943791, 1.79175947, 1.94591015, 2.07944154, 2.19722458])
'''
```

## Estadísticas

```py
arr = np.arange(0,10)
# array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
arr.sum()
# 45
np.mean()
# 4.5
arr.max()
# 9
arr.min()
# 0
arr.var() # Varianza
# 8.25
arr.std() # 
# 2.8722813232690143
```

## Lógica de ejes

```py
arr_2d = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])
'''
array([[ 1,  2,  3,  4],
       [ 5,  6,  7,  8],
       [ 9, 10, 11, 12]])
'''
arr_2d.sum(axis=0)
# array([15, 18, 21, 24])
arr_2d.sum(axis=1)
# array([10, 26, 42])
```

![Lógica de ejes](./img/axis_logic.png)
