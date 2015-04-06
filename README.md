# glsl-halftone

[![stable](http://badges.github.io/stability-badges/dist/stable.svg)](http://github.com/badges/stability-badges)

![halftone](http://i.imgur.com/GTjatxC.jpg)

[(glslbin demo)](http://glslb.in/s/4d6e366d)

A halftone shader in GLSL, adapted from Stefan Gustavson's work [here](http://webstaff.itn.liu.se/~stegu/webglshadertutorial/shadertutorial.html). Bilinear texture sampling and minification are left up to the user to implement (see steps 5 and 9 in Gustavson's tutorial).

For anti-aliasing, you should enable standard derivatives at the top of your shader, before requiring this module.

```glsl
precision highp float;

#ifdef GL_OES_standard_derivatives
#extension GL_OES_standard_derivatives : enable
#endif

varying vec2 uv;
uniform vec2 iResolution;
uniform sampler2D u_sampler;

#pragma glslify: halftone = require('glsl-halftone')

void main() {
  //sample from texture; optionally using manual bilinear filtering
  vec4 texcolor = texture2D(u_sampler, uv);

  //aspect corrected texture coordinates
  vec2 st = uv;
  st.x *= iResolution.x / iResolution.y;
  
  //apply halftone effect
  gl_FragColor.rgb = halftone(texcolor.rgb, st);
  gl_FragColor.a = 1.0;
}
```

## Usage

[![NPM](https://nodei.co/npm/glsl-halftone.png)](https://www.npmjs.com/package/glsl-halftone)

#### `vec3 halftone(vec3 color, vec2 uv[, float frequency])`

Applies a halftone effect to the given texture color using the `uv` coordinates. Higher `frequency` (default 30.0) leads to more tiling and smaller circles. Returns the RGB result. 

## Contributing

See [stackgl/contributing](https://github.com/stackgl/contributing) for details.

## License

The halftone effect is public domain, by Stefan Gustavson. This also uses Ashima's glsl-noise, which is MIT.

The rest of the adaptation is MIT, see [LICENSE.md](http://github.com/stackgl/glsl-halftone/blob/master/LICENSE.md) for details.
