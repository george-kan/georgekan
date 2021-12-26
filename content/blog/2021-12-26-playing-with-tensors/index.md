---
categories:
- deep learning
date: "2021-12-26"
draft: false
excerpt: Basic tensor manipulation.
layout: single
title: Playing with tensors
---

This is my third try at understanding how to work with deep learning and in particular PyTorch. I am following the [fastai](https://www.fast.ai/) course structure and I am hoping for better results this time 😅. The first step in the journey is to work a bit with the fundamental building block of PyTorch, **tensors**.


```python
import torch
```

## 1. Let's create some tensors

We first create a 3x5 tensor with all values equal to 1.
```python
t1 = torch.ones([3,5])
t1
```




    tensor([[1., 1., 1., 1., 1.],
            [1., 1., 1., 1., 1.],
            [1., 1., 1., 1., 1.]])




```python
# Properties
print((t1.shape, t1.type()))
```

    (torch.Size([3, 5]), 'torch.FloatTensor')
    


```python
# Notice the difference between the standard type and the tensor method type
print(type(t1), t1.type())
```

    <class 'torch.Tensor'> torch.FloatTensor
    

## 2. Perform standard operations


```python
print((t1 * 5)**2)

print(t1+2)
```

    tensor([[25., 25., 25., 25., 25.],
            [25., 25., 25., 25., 25.],
            [25., 25., 25., 25., 25.]])
    tensor([[3., 3., 3., 3., 3.],
            [3., 3., 3., 3., 3.],
            [3., 3., 3., 3., 3.]])
    

## 3. Where does the tensor live?
As you know, pytorch can make use of the **GPU** in order to train models faster. But in order to do so, the objects it is working on have to also be on the GPU. By default tensors are created on the **CPU** and therefore they have to be moved.


```python
# Checking which devices are available 
print('Is a GPU available:' + str(torch.cuda.is_available()))

# How many are available
print('There is ' + str(torch.cuda.device_count()) + ' GPU available')

# What is its name
print('Its name is ' + str(torch.cuda.get_device_name()))

# Let's assign the device
device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")

# Notice the numbering here, we specify cuda:0. If you have multiple cudas you can specify cuda:1 etc.  
```

    Is a GPU available:True
    There is 1 GPU available
    Its name is Tesla K80
    


```python
# Checking the current device of the tensor
print('Our tensor current lives in the: ' + str(t1.device))

# Ways to change the device 
t2 = t1.cuda()
t3 = torch.ones([3,5], device = device)
t4 = t1.to(device)

print(t1.device, t2.device, t3.device, t4.device)
```

    Our tensor current lives in the: cpu
    cpu cuda:0 cuda:0 cuda:0
    

As you might have noticed, none of the operations we performed modified t1 in place. We needed to assign the new tensor every time. 
A second question I had was whether we could perform operations with tensors on different devices.


```python
t1 + t2

```


    ---------------------------------------------------------------------------

    RuntimeError                              Traceback (most recent call last)

    <ipython-input-10-9ac58c83af08> in <module>()
    ----> 1 t1 + t2
    

    RuntimeError: Expected all tensors to be on the same device, but found at least two devices, cuda:0 and cpu!


Turns out that we cannot! 😄

## 4. Spatial manipulations of tensors

*Transforming a tensor into a long 1 dimensional tensor*
```python
print(t1.flatten())
print('Flattened shape:' + str(t1.flatten().shape))
```

    tensor([1., 1., 1., 1., 1., 1., 1., 1., 1., 1., 1., 1., 1., 1., 1.])
    Flattened shape:torch.Size([15])
    

*Stacking tensors*  
*This is a really cool operation, it takes a list/tuple of tensors and creates a tensor with an extra dimension that contains all*
```python
of them
stacked_tensor = torch.stack((t1, t1)) #torch.stack((t1, t1)) also works
print('Initial shape of the tensors: ' + str(t1.shape))
print('Stacked tensor shape: ' + str(stacked_tensor.shape))
```

    Initial shape of the tensors: torch.Size([3, 5])
    Stacked tensor shape: torch.Size([2, 3, 5])
    

## 5. What differentiates a tensor from a `numpy` array?  
* They can be sent and processed by the GPU
* They can "remember" computations performed on them to compute gradient


```python
# Notice the decimal point next to 1, this forces the tensor to be a FloatTensor and not a LongTensor
t5 = torch.tensor([1.,2,3], requires_grad=True, device=device)
t5

print('The gradients of t5 are: ' + str(t5.grad))
```

    The gradients of t5 are: None
    

By setting `requires_grad=True` we indicate to PyTorch that it should remember the computations performed on this tensor.  
Once we are done with the computations, in order to trigger the derrivative calculations we have to use the `.backward` method on the last computation step.


```python
# Testing by raising to the second power
y = (t5**2).sum()
y.backward()
print(t5.grad)
```

    tensor([2., 4., 6.], device='cuda:0')
    

### Time for some math! 😃
We defined y as follows: $$ y = x_1^2+x_2^2+x_3^2 $$  
When we use `.backward` we are telling PyTorch to calculate the following:
$$ \large \frac {\partial{y}} {\partial{x_1}}(x_1), \frac {\partial{y}} {\partial{x_2}}(x_2), \frac {\partial{y}} {\partial{x_3}}(x_3)$$  
If you remember your calculus (if not just take my word for it!):  
$$ \large \frac {\partial{y}} {\partial{x_1}} = 2x_1 \rightarrow \frac {\partial{y}} {\partial{x_1}}(x_1)=2*1=2$$  
The same logic applies to the other two.

You might have noticed that I did not simply square `x` but I summed up the results. Why is that?  
Well, if I simply took the square of `x` I would need to specify to the `backward` method the gradient of y with respect to y. Let's see this in practice:


```python
t5.grad = None #Resetting the gradient
print(t5.grad)
```

    None
    


```python
y2 = (t5**2)
y2.backward()
print(t5.grad)
```


    ---------------------------------------------------------------------------

    RuntimeError                              Traceback (most recent call last)

    <ipython-input-16-2d312ea1c577> in <module>()
          1 y2 = (t5**2)
    ----> 2 y2.backward()
          3 print(t5.grad)
    

    /usr/local/lib/python3.7/dist-packages/torch/_tensor.py in backward(self, gradient, retain_graph, create_graph, inputs)
        305                 create_graph=create_graph,
        306                 inputs=inputs)
    --> 307         torch.autograd.backward(self, gradient, retain_graph, create_graph, inputs=inputs)
        308 
        309     def register_hook(self, hook):
    

    /usr/local/lib/python3.7/dist-packages/torch/autograd/__init__.py in backward(tensors, grad_tensors, retain_graph, create_graph, grad_variables, inputs)
        148 
        149     grad_tensors_ = _tensor_or_tensors_to_tuple(grad_tensors, len(tensors))
    --> 150     grad_tensors_ = _make_grads(tensors, grad_tensors_)
        151     if retain_graph is None:
        152         retain_graph = create_graph
    

    /usr/local/lib/python3.7/dist-packages/torch/autograd/__init__.py in _make_grads(outputs, grads)
         49             if out.requires_grad:
         50                 if out.numel() != 1:
    ---> 51                     raise RuntimeError("grad can be implicitly created only for scalar outputs")
         52                 new_grads.append(torch.ones_like(out, memory_format=torch.preserve_format))
         53             else:
    

    RuntimeError: grad can be implicitly created only for scalar outputs


To fix the error above we need to pass in to `backward` a tensor of the same size as y with values equal to $$ \large \frac {\partial{y_2}} {\partial{y_2}} = 1$$. 


```python
y2 = (t5**2)
y2.backward(gradient = torch.tensor([1.,1.,1.], device=  device))
print(t5.grad)
```

    tensor([2., 4., 6.], device='cuda:0')
    

The gradients are the same as before.

## 6. Consecutive computations
I was also curious to see how these operations work when we have multiple computations. Let's test this out:


```python
t5.grad = None #Resetting the gradient
y = (t5**2)
z = 3*y.sum()
z.backward()
print(t5.grad)
```

    tensor([ 6., 12., 18.], device='cuda:0')
    

Everything works as expected!  
We defined $$ z = 3y_1 + 3y_2 + 3y_3$$ and through the `backward` method we calculate the following $$ \large \frac {\partial{z}} {\partial{x_1}}(x_1), \frac {\partial{z}} {\partial{x_2}}(x_2), \frac {\partial{z}} {\partial{x_3}}(x_3)$$. As an example:  
$$ \large \frac {\partial{z}} {\partial{x_1}} = \frac {\partial{z}} {\partial{y_1}}\frac {\partial{y_1}} {\partial{x_1}} = 3*2x_1$$
Evaluated at $$x_1=1$$ this results in the 6 we see above.
