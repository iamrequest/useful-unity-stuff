# Useful Math

In general, you can search "[math term] Freya Holmér" to find a simple explanation of most math concepts.

## Dot Product

The dot product tells us whether or not two vectors are pointing in the same direction.

[Freya Holmér: Dot Product Visual](https://twitter.com/FreyaHolmer/status/1200807790580768768)

The dot product can be calculated like so:

```
dot product = a ⋅ b
= |a| * |b| * cos(θ)
= (a.x * b.x + a.y * b.y) * cos(θ)

Where,
	a ⋅ b represents the dot product of vectors a and b
	|a| represents the length (magnitude) of vector a
	θ represents the angle between vectors a and b, in radians
```

In Unity, you can calculate the dot product like so:

```cs
Vector3 a;
Vector3 b;

float dotProduct = Vector3.Dot(a, b);
```


## Cross Product

The cross product gives us a vector that is orthagonal (perpendicular) to the two input vectors.


The cross product can be calculated on a per-component basis like so:

```
c = a × b

c.x = a.y * b.z + a.z * b.y
c.y = a.z * b.x + a.x * b.z
c.z = a.x * b.y + a.y * b.x

Where,
	a × b represents the cross product of vectors a and b
```

In Unity, you can calculate the cross product like so:

```cs
Vector3 a;
Vector3 b;

float crossProduct = Vector3.Cross(a, b);
```


## Lerp

Lerp (Linear Interpolation) is used to blend between two input values.

[Freya Holmér: Lerp Visual](https://twitter.com/FreyaHolmer/status/1175925033002254338)

Given two input values (`a`, `b`) and a percentage value (`t`), `Lerp(a, b, t)` will return a value that is `t`% between `a` and `b`.


Lerp can be calculated via:

```
lerp(a, b, t)
= (1 - a) * t + b * t
```

In Unity, you can use lerp on a per-context basis:

```
// Lerp(a, b, t);

Vector2.Lerp();
Vector3.Lerp();
Color.Lerp();
Mathf.Lerp(); // Floats
```

Slerp (Spherical Lerp) is also a thing, which is the same thing only it interpolates in a circular pattern instead of a straight line. Useful for quaternions and direction vectors.

[Freya Holmér: Slerp Visual](https://twitter.com/FreyaHolmer/status/1176137498323501058)

```cs
// Slerp(a, b, t);

Quaternion.Lerp();
```


## Inverse Lerp

Inverse Lerp is used to find how far some value `v` exists between input values `a` and `b`. Given these 3 input values, we will get a return float value `t`.

Note: The return value of InverseLerp() is not clamped between [0, 1].

Note: If `b` and `a` are the same value, you'll get a divide by zero error.

```
t = inverseLerp(a, b, v);
t = (v - a) / (b - a)
```

In Unity, you can use InverseLerp() on floats like below.

```cs
float a, b, v;
float t = Mathf.InverseLerp(a, b, v);
```

Freya Holmér's [mathfs library](https://github.com/FreyaHolmer/Mathfs) has some more InverseLerp() functions defined:

```cs
float t = Mathfs.InverseLerp(a, b, v);

// Clamps t between [0, 1]
float t = Mathfs.InverseLerpClamped(a, b, v);
```
