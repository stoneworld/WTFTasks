# Google Form 测验
## 数据实时同步方案

1. 创建一个 google form quiz 表单:

<img src=assets/google_form_1.png width=50% />

2. 增加 FormSubmit 触发器

如图在表单编辑页面点击脚本编辑器。

<img src=assets/google_form_script.png width=50% />

在编辑器区域新增一个代码文件，示例如下：

```js
function onFormSubmit(e) {
    var form = FormApp.getActiveForm();
    const id = form.getId();
    const key = form.getTitle();
    const r = e.response;
    const email = r.getRespondentEmail();
    let totalScore = 0;
    r.getGradableItemResponses().forEach((item) => {
        totalScore += item.getScore();
    });
    // 你需要的数据参数
    let params = {}
    var url = "xxx"; // 自己的服务器地址
    var options = {
        "method": "post",
        "headers": {
            "Content-Type": "application/json"
        },
        "payload": JSON.stringify(params)
    };
    
    var response = UrlFetchApp.fetch(url, options);

    var result = JSON.parse(response.getContentText())

    console.log(result.code)

}
```

而后创建一个代码触发器，示例图如下：

<img src=assets/google_form_trigger.png width=50% />

在脚本执行的位置可以看到每次触发的明细日志，有问题可以在这里排查。

<img src=assets/google_form_trigger_log.png width=50% />

3. 服务端处理数据

服务端接收到 google form 的数据，需要将用户的成绩记录下来，这样整个成绩同步流程就结束了。