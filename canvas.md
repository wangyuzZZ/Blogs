# canvas

## 1.canvas 图片水印

```javascript

shuiyin({
        url: fileName.dataURL, //这里传的file是上传的图片文件(最好用base64)
        content: "仅用威选平台资料认证，他用无效",//水印文字
        textAlign: "left",//文字起始位置
        textBaseline: "top",//文字起始位置
        font: "12px Microsoft Yahei",//文字大小
        fillStyle: "rgba(0,0,0,.5)"//文字验收
    )}
shuiyin(obj) {
      let chr = obj.content.split("");
      let temp = "";
      let row = [];
      const img = new Image();
      img.src = obj.url;
      img.crossOrigin = "anonymous";
      img.onload = function () {
        const canvas = document.createElement("canvas");
        let _ix = img.width;//获取图片宽度
        let _iy = img.height;//获取图片高度
        canvas.width = _ix;//设置canvas宽度
        canvas.height = _iy;//设置canvas高度
        const ctx = canvas.getContext("2d");
        ctx.drawImage(img, 0, 0);//绘制图片（先绘制会置于图层底层）
        ctx.textAlign = obj.textAlign;
        ctx.textBaseline = obj.textBaseline;
        ctx.font = obj.font;
        ctx.fillStyle = obj.fillStyle;
        ctx.translate(0, 0);
        ctx.rotate(-20*Math.PI/180);//旋转角度
        for (let i = 0; i < _iy / 120; i++) {
           for (let j = 0; j < _ix / 50; j++) {
             ctx.fillText(obj.content, i * 120, j * 50, _ix);
          }
        }
        const base64Url = canvas.toDataURL();
        obj.cb && obj.cb(base64Url);
      };
    },
```

## 2.图片水印（效果较好）

```js
// 水印
export default class Watermark {
  // 初始化水印画板
  init(opations) {
    let obj = {
      text: '仅用威选平台资料认证，他用无效', //水印文字
      imgUrl: '', //图片地址
      font: '20px 黑体', //字体样式
      fillStyle: 'rgba(0,0,0,0.1)', //字体颜色
      rotateAngle: -20 * Math.PI / 180, //旋转角度
    }
    Object.assign(obj, opations); //合并参数
    const cw = document.createElement("canvas"); //生成水印文字
    var ctx = cw.getContext("2d"); //返回一个用于在画布上绘图的环境
    ctx.clearRect(0, 0, cw.width, cw.height); //绘制之前画布清除
    ctx.font = obj.font;
    ctx.rotate(obj.rotateAngle);
    ctx.fillStyle = obj.fillStyle;
    ctx.fillText(obj.text, -20, 120);
    ctx.rotate(-obj.rotateAngle); //坐标系还原
    this.render(obj, cw); //将水印跟图片绘制到新画板中
  }
  // 生成水印
  render(obj, cw) {
    const img = new Image();
    img.src = obj.imgUrl;
    img.crossOrigin = "anonymous";
    img.onload = function () {
      const canvas = document.createElement("canvas");
      const ctxr = canvas.getContext("2d");
      let _ix = img.width;
      let _iy = img.height;
      canvas.width = _ix;
      canvas.height = _iy;
      ctxr.clearRect(0, 0, canvas.width, canvas.height); //清除整个画布 
      ctxr.drawImage(img, 0, 0);
      var pat = ctxr.createPattern(cw, "repeat"); //在指定的方向上重复指定的元素 
      ctxr.fillStyle = pat;
      ctxr.fillRect(0, 0, canvas.width, canvas.height);
      const base64Url = canvas.toDataURL();
      obj.success && obj.success(base64Url);
    };
  }
}

```

