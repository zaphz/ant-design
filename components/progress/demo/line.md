# 进度条

- order: 0

标准的进度条。

---

````jsx
var Progress = antd.Progress.Line;

React.render(
  <div>
    <Progress percent="30" />
    <Progress percent="70" status="exception" />
    <Progress percent="100" />
  </div>
  , document.getElementById('components-progress-demo-line'));
````
