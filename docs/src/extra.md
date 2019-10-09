# Miscellaneous Tools

There are many handy tools implemented in `ADCME` for analysis, benchmarking, input/output, etc. 

## Debugging and printing

Add the following line before `Session` and change `tf.Session` to see verbose printing (such as GPU/CPU information)
```julia
tf.debugging.set_log_device_placement(true)
```

`tf.print` can be used for printing tensor values. It must be binded with an executive operator.
```julia
# a, b are tensors, and b is executive
op = tf.print(a)
b = bind(b, op)
```

## Benchmarking

The functions [`tic`](@ref) and [`toc`](@ref) can be used for recording the runtime between two operations. `tic` starts a timer for performance measurement while `toc` marks the termination of the measurement. Both functions are bound with one operations. For example, we can benchmark the runtime for `svd`

```julia
A = constant(rand(10,20))
A = tic(A)
r = svd(A)
B = r.U*diagm(r.S)*r.Vt 
B, t = toc(B)
run(sess, B)
run(sess, t)
```

```@docs
tic
toc
```

## Save and Load Python Object
```@docs
psave
pload
```

## Save and Load TensorFlow Session
```@docs
load
save
```

## Save and Load Diary

We can use TensorBoard to track a scalar value easily
```julia
d = Diary("test")
p = placeholder(1.0, dtype=Float64)
b = constant(1.0)+p
s = scalar(b, "variable")
for i = 1:100
    write(d, i, run(sess, s, Dict(p=>Float64(i))))
end
activate(d)
```

```@docs
Diary
scalar
activate
load
save
write
```