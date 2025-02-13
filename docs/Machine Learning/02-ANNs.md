# Artificial Neural Networks (ANNs)

## Modelo del perceptron (Perceptron model)

```mermaid
flowchart LR
subgraph inputs
x1[x1]
x2[x2]
end

fx(("f(X)"))

subgraph Output
y[y]
end

x1 -- w1+b--> fx
x2 -- w2+b --> fx
fx --> y
```

`w: weight`

`b: bias`

>$\hat{y} = \sum_{i=1}^{n} x_iw_i + b_i$

and if B = b1+b2+...+bn

>$\hat{y} = B + \sum_{i=1}^{n} x_iw_i$
