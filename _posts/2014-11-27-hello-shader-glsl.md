---
layout: post
title:  Cocos Lua脚本中使用shader
category: "游戏研发"
tag:  "Cocos"
---
本文只介绍Cocos Lua中如何使用Shader，不会介绍如何编写Shader代码，以及原理性的东西。（因为我现在也不懂）

本文代码实现的效果为一张图片慢慢的从无到有，从上面一点一点的显示。

###在初始化时设置Shader

```
	-- 设置shader
	function TestShader:setShader()
	    local shader = cc.GLProgram:createWithFilenames(vsh, fsh)
	    shader:bindAttribLocation(cc.ATTRIBUTE_NAME_POSITION, cc.VERTEX_ATTRIB_POSITION)
	    shader:bindAttribLocation(cc.ATTRIBUTE_NAME_COLOR, cc.VERTEX_ATTRIB_COLOR)
	    shader:bindAttribLocation(cc.ATTRIBUTE_NAME_TEX_COORD, cc.VERTEX_ATTRIB_TEX_COORDS)
	    shader:link()
	    self:setGLProgram(shader)
	end
	
```

函数首先根据vsh和fsh顶点着色器和片段着色器创建一个Shader，然后bindAttribLocation函数绑定相关的顶点属性。
bindAttribLocation使用glBindAttribLocation函数，参数为cocos定义的默认顶点属性，cocos中定义的顶点属性包括：

```

	// Attribute names
	const char* GLProgram::ATTRIBUTE_NAME_COLOR = "a_color";
	const char* GLProgram::ATTRIBUTE_NAME_POSITION = "a_position";
	const char* GLProgram::ATTRIBUTE_NAME_TEX_COORD = "a_texCoord";
	const char* GLProgram::ATTRIBUTE_NAME_NORMAL = "a_normal";
	const char* GLProgram::ATTRIBUTE_NAME_BLEND_WEIGHT = "a_blendWeight";
	const char* GLProgram::ATTRIBUTE_NAME_BLEND_INDEX = "a_blendIndex";

	 enum
	    {
	        VERTEX_ATTRIB_POSITION,
	        VERTEX_ATTRIB_COLOR,
	        VERTEX_ATTRIB_TEX_COORD,
	        VERTEX_ATTRIB_NORMAL,
	        VERTEX_ATTRIB_BLEND_WEIGHT,
	        VERTEX_ATTRIB_BLEND_INDEX,

	        VERTEX_ATTRIB_MAX,

	        // backward compatibility
	        VERTEX_ATTRIB_TEX_COORDS = VERTEX_ATTRIB_TEX_COORD,
	    };

```

最后编译链接Shader，即可。


###在update过程，使用shader

```
	function TestShader:useShader()
	    self:getGLProgram():use()
	    local state = self:getGLProgramState()
	    state:setUniformFloat('u_showRate', self.showRate)
	end

```
使用的过程中，设置shader的uniform变量


###顶点着色器
顶点着色器输出gl_Position，以及为片段着色器所使用的varying赋值。

```

	attribute vec4 a_position;
	attribute vec2 a_texCoord;
	attribute vec4 a_color;

	#ifdef GL_ES
	varying lowp vec4 v_fragmentColor;
	varying mediump vec2 v_texCoord;
	#else
	varying vec4 v_fragmentColor;
	varying vec2 v_texCoord;
	#endif

	void main()
	{
	    gl_Position = CC_PMatrix * a_position;
	    v_fragmentColor = a_color;
	    v_texCoord = a_texCoord;
	}

```

###片段着色器

片段着色器中的varying为顶点着色器设置之后，插值获得。

```

	#ifdef GL_ES
	precision lowp float;
	#endif

	varying vec4 v_fragmentColor;
	varying vec2 v_texCoord;
	uniform float u_showRate;

	void main()
	{
		vec4 color = vec4(0.0);
		if( v_texCoord.y < u_showRate )
		{
			color = texture2D(CC_Texture0, v_texCoord);
		}
		else{
			color.a = 0;
		}

		gl_FragColor = color;
	}

```