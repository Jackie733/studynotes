# Javascript优化

Just In Time(JIT)编译器

不必去绞尽脑汁的思考微优化（例如for循环和while循环到底哪个快）

### requestAnimationFrame

```javascript
function animate() {
  //Do something super rad here.
  requestAnimationFrame(animate);
}
requestAnimationFrame(animate);
```

测试用~~~~