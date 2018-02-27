# Kaptcha 详细配置表

---

<table style="color:rgb(54,46,43);font-family:Arial;font-size:14px;line-height:26px;">
<tbody>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);"><strong>Constant</strong></td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);"><strong>描述</strong></td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);"><strong>默认值</strong></td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.border</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">图片边框，合法值：yes , no</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">yes</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.border.color</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">边框颜色，合法值： r,g,b (and optional alpha) 或者 white,black,blue.</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">black</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.border.thickness</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">边框厚度，合法值：&gt;0</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">1</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.image.width</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">图片宽</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">200</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.image.height</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">图片高</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">50</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.producer.impl</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">图片实现类</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">com.google.code.kaptcha.impl.DefaultKaptcha</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.textproducer.impl</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">文本实现类</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">com.google.code.kaptcha.text.impl.DefaultTextCreator</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.textproducer.char.string</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">文本集合，验证码值从此集合中获取</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">abcde2345678gfynmnpwx</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.textproducer.char.length</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">验证码长度</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">5</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.textproducer.font.names</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">字体</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">Arial, Courier</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.textproducer.font.size</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">字体大小</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">40px.</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.textproducer.font.color</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">字体颜色，合法值： r,g,b &nbsp;或者 white,black,blue.</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">black</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.textproducer.char.space</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">文字间隔</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">2</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.noise.impl</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">干扰实现类</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">com.google.code.kaptcha.impl.DefaultNoise</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.noise.color</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">干扰&nbsp;颜色，合法值： r,g,b 或者 white,black,blue.</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">black</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.obscurificator.impl</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">图片样式：&nbsp;<br /> 水纹com.google.code.kaptcha.impl.WaterRipple&nbsp;<br /> 鱼眼com.google.code.kaptcha.impl.FishEyeGimpy<br /> 阴影com.google.code.kaptcha.impl.ShadowGimpy</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">com.google.code.kaptcha.impl.WaterRipple</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.background.impl</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">背景实现类</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">com.google.code.kaptcha.impl.DefaultBackground</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.background.clear.from</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">背景颜色渐变，开始颜色</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">light grey</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.background.clear.to</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">背景颜色渐变，结束颜色</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">white</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.word.impl</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">文字渲染器</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">com.google.code.kaptcha.text.impl.DefaultWordRenderer</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.session.key</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">session key</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">KAPTCHA_SESSION_KEY</td> 
 </tr>
 <tr>
  <td style="padding:5px;border:1px solid rgb(204,204,204);">kaptcha.session.date</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">session date</td> 
  <td style="padding:5px;border:1px solid rgb(204,204,204);">KAPTCHA_SESSION_DATE</td> 
 </tr>
</tbody>
</table> 