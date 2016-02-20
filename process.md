* What code is better: implicit (+) or explicit (-)? E. g. create unfolded object for each structure instance or provide type constructor?
	* + implicit is able to be unfolded - closurecompiler or ast-eval
	* - implicit is sometimes senseless like var a = bool();
		* + same time it may provide different implementation of bool.
	* + implicit way avoids unfolding matrices multiplication: `mat2d['*'](a, b)`
	* - explicit is easier to read
	* - explicit corresponds to swizzles
	* - implicit may be slower in simple cases like `var a = false`
		* + but times faster in multiple use like `var a = mat4(3)`
	* - in some cases implicits are N times slower: `xy.xy *= uv.yx;`. Expanding to explicit almost take no time to calculate `xy[0] *= uv[0]; xy[1] *= uv[1];`, and also concise. But dealing with types is cumbersome: `vec2.mult(vec2.xy(xy), vec2.yx(uv), xy);`
		* + at the same way, explicits are over-calculative, eg `a.xy *= b.x / b.y` — for each component of `a` we calculate `b.x/b.y`, and if it is a call for some troublesome method, it will be called up to 16 times (`mat4 = meth()`). Whereas with implicit call it will calculate `b.x/b.y` once and do assignment.
	* + From the other POV, who on earth cares about the beauty of compiled code?
	* ✔ The good compromise is found in extending objects returned from types with swizzles etc. Takes more time on object constructs, but resulting code is pretty: `m = m.mult(n);`
		* Unfortunately, we have to extend native arrays prototype to keep execution fast enough.