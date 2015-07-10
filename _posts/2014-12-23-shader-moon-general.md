---
layout: post
title:  在Cocos中使用shader实现月相变化
category: "游戏研发"
tag:  "Cocos"
---

##分析

要实现月相变化，有三点需要注意：

- 月亮实体。也就是说用一个图片表示月亮
- 月亮的光晕。月亮周边的光晕可以通过图片表现，但是如果是月相，被遮挡住的部分也需要有光晕。
- 被遮挡部分

最终月亮效果如下：
![](/images/2014/20141223132514.png)

###遮挡的实现

定义一个遮挡的圆形（遮罩），这个圆形和月亮（包括光晕）的圆形大小一样，而要使月相变化只需要移动这个遮罩的位置。
如月相为0（月亮完全被盖住），只需要将遮罩完全盖住月亮。
月相为50，将遮罩的圆心定义在月亮圆形的边上。
月相为100，将遮罩拿开

###光晕的实现

光晕也分为三部分处理。

1.月亮圆形边上的光晕。

2.月亮豁口的光晕（遮罩的光晕）。

3.月亮豁口和圆形边交界的光晕。

光晕使用如下方法来模拟：距离月亮实体越近，透明度越低。

##片段着色器

	//功能：遮罩区域不显示，其余部分显示。
	#ifdef GL_ES
	precision lowp float;
	#endif
	
	varying vec4 v_fragmentColor;
	varying vec2 v_texCoord;
	
	uniform vec2 u_maskCenter;  // 遮罩中心
	uniform float u_maskRadius;  // 遮罩半径
	uniform float u_moonRadius;  // 月亮半径
	
	void main()
	{
		--该点到遮罩圆心的距离
		float x2_mask = (v_texCoord.x - u_maskCenter.x) * (v_texCoord.x - u_maskCenter.x);
		float y2_mask = (v_texCoord.y - u_maskCenter.y) * (v_texCoord.y - u_maskCenter.y);
		--该点到月亮圆心的距离
		float x2_center = (v_texCoord.x - 0.5) * (v_texCoord.x - 0.5);
		float y2_center = (v_texCoord.y - 0.5) * (v_texCoord.y - 0.5);
		--遮罩和月亮实体的半径，注意，遮罩的半径和月亮光晕的半径是相等的。
		float r2_moon = u_moonRadius * u_moonRadius;
		float r2_mask = u_maskRadius * u_maskRadius;
		
		if (x2_mask + y2_mask >= r2_mask && x2_center + y2_center < r2_moon)//（月亮实体处理）像素点在月亮实体内且在遮罩外
		{
			gl_FragColor =  texture2D(CC_Texture0, v_texCoord);
		}
		else if(x2_mask + y2_mask >= r2_moon && x2_center + y2_center < r2_mask)//（光晕）点在月亮的光晕中且在遮罩的光晕中
		{
			float ratio;
			float r1 = 1- (sqrt(x2_center + y2_center) - u_moonRadius) / (u_maskRadius - u_moonRadius);//月亮光晕
			float r2 = 1- (u_maskRadius - sqrt(x2_mask + y2_mask)) / (u_maskRadius - u_moonRadius);//遮罩光晕
			if (x2_mask + y2_mask >= r2_mask && x2_center + y2_center >= r2_moon)
				ratio = r1;
			else if (x2_center + y2_center < r2_moon) 
				ratio = r2;
			else
				ratio = r1 * r2; //月角，这个地方比较难理解
			
			vec4 color = texture2D(CC_Texture0, vec2(0.5, 0.5));//颜色定义为月亮图片中间的
			color.a = pow(ratio, 3.0);//透明度变化更快
			gl_FragColor = color;
		}
		else//其他地方为背景
		{
			gl_FragColor = vec4(0.0);
		}
	}


##Shader的使用

	-- 可控制月相变换的月亮。
	local general_moon = class('general_moon', function()
	    return cc.Node:create()
	end)
	
	local vsh = 'default.vsh'
	local fsh_main = 'general_moon_main.fsh'
	
	local tex_main = 'tmp/moon.png'
	
	-- 月相范围
	local max_ph = 100
	local min_ph = 0
	local def_ph = 50
	
	 -- 阴影半径
	local mask_radius = 125
	
	-- 月亮半径
	local moon_radius = 100
	
	
	-- 创建函数
	function general_moon.create(phase)
	    local node = general_moon.new()
	    node:init(phase)
	    return node
	end
	
	-- 初始化
	function general_moon:init(phase)
	    self:initSprite()-- 初始化sprite
	    self.phase = (phase or def_ph)-- 设置月相
	    self:initShader()-- 初始化shader
	    cc.scheduleUpdate(self, self.update)
	end
	
	-- 渐变月相
	function general_moon:changePhase(phase)
	    self.phase = phase
	end
	
	-- 更新
	function general_moon:update(dt)
	        self:useShader() 
	end
	
	-- 初始化sprite
	function general_moon:initSprite()
	    local moonMain = cc.Sprite:create(tex_main)
	    moonMain:setBlendFunc(gl.SRC_ALPHA, gl.ONE_MINUS_SRC_ALPHA)
	    self:addChild(moonMain)
	    self._moonMain = moonMain
	    self._maskRadius = mask_radius / moonMain:getContentSize().width
	    self._moonRadius = moon_radius / moonMain:getContentSize().width
	end
	
	
	-- 初始化shader
	function general_moon:initShader()
	    local shader_main = cc.GLProgram:createWithFilenames(vsh, fsh_main)
	    shader_main:bindAttribLocation(cc.ATTRIBUTE_NAME_POSITION, cc.VERTEX_ATTRIB_POSITION)
	    shader_main:bindAttribLocation(cc.ATTRIBUTE_NAME_COLOR, cc.VERTEX_ATTRIB_COLOR)
	    shader_main:bindAttribLocation(cc.ATTRIBUTE_NAME_TEX_COORD, cc.VERTEX_ATTRIB_TEX_COORDS)
	    shader_main:link()
	    self._moonMain:setGLProgram(shader_main)
	end
	
	-- 使用shader
	function general_moon:useShader()
	    self._moonMain:getGLProgram():use()
	    local maskCenterMain = 0.5
	        - math.sqrt(2) * self._maskRadius * (self.phase  - min_ph) / (max_ph - min_ph)
	    local stateMain = self._moonMain:getGLProgramState()
	    stateMain:setUniformVec2('u_maskCenter', cc.p(maskCenterMain, maskCenterMain))
	    stateMain:setUniformFloat('u_moonRadius', self._moonRadius)
	    stateMain:setUniformFloat('u_maskRadius', self._maskRadius)
	
	end

	return general_moon

解释：

1.根据月相比例确定遮罩的圆心，纹理坐标从左上角开始，因为这个坐标只是x和y分量的坐标，所以要乘以sqrt(2)
	local maskCenterMain = 0.5 - math.sqrt(2) * self._maskRadius * (self.phase  - min_ph) / (max_ph - min_ph)

2.传给shader遮罩的圆心、遮罩半径和月亮实体半径。注意，**这里遮罩半径==月亮光晕半径**

3.moonMain:setBlendFunc(gl.SRC_ALPHA, gl.ONE_MINUS_SRC_ALPHA)这个是默认的，但是这里不知道为什么没有这句话就不显示。

