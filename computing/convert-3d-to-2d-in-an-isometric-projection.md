# Convert 3D to 2D in an isometric projection

#maths #games #isometric #c #csharp #javascript

-----

The simplified calculation to transform a 3D point to 2D in an isometric project is:

```latex
\begin{pmatrix}
x' \\
y'
\end{pmatrix}
=
\left( \begin{array}{c}
x - z \\
\frac{x}{2} + \frac{z}{2} - y
\end{array} \right)
```

Which can be expressed as:

```c
(x - z, x / 2 + z / 2 - y)
```

To transform a 3D axis-aligned bounding box (AABB), apply the same calculation to its corners and return the min/max values.

```c
corners = isometric(box3D.corners)
return {
  x1 = low(corners.x)
  y1 = low(corners.y)
  x2 = high(corners.x)
  y2 = high(corners.y)	
}
```

## Implementations

### JavaScript

```javascript
function pointToIsometric(point) {
	return {
		x: x - z,
		y: x / 2 + z / 2 - y
	};
}
```

### C# MonoGame Vector3 --> Vector2

```csharp
using Microsoft.Xna.Framework;

public static Vector2 ToIsometric(this Vector3 pt)
{
		return new Vector2(pt.X - pt.Z, (pt.X / 2) + (pt.Z / 2) - pt.Y);
}
```

### C# MonoGame BoundingBox --> Rectangle

This example depends on the `ToIsometric()` method above

```csharp
using Microsoft.Xna.Framework;

public static Rectangle ToIsometric(this BoundingBox box)
{
	int x1 = int.MaxValue;
	int x2 = int.MinValue;
	int y1 = int.MaxValue;
	int y2 = int.MinValue;
	Vector3[] corners = bounds.GetCorners();

	for (int i = 0; i < 8; i++)
	{
			Vector2 c = corners[i].ToIsometric();
			x1 = (int)Math.Min(x1, c.X);
			x2 = (int)Math.Max(x2, c.X);
			y1 = (int)Math.Min(y1, c.Y);
			y2 = (int)Math.Max(y2, c.Y);
	}

	return new Rectangle(x1, y1, x2 - x1, y2 - y1);
}
```
