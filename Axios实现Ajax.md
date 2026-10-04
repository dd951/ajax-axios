1、第三方库，底层基于 Promise+XHR 封装，并不是对 Fetch API 的封装，比 Fetch API 好用。使用它需要引入 `axios.js`库文件。使用国内 CDN 加速引入：

```html
<script src="https://cdn.bootcdn.net/ajax/libs/axios/1.6.7/axios.min.js"></script>
```

2、发送 get 请求

```javascript
axios.get('check', {
    // 这个参数将被自动追加到url后面，格式为 usrl?name=value
    params: {username:username},
	// 发送请求时这个将被自动放到请求头中
    headers: {a:"b"}
}).then(reponse => {
    if(reponse.data.success) {
        showFeedback('success', '恭喜你，您的用户名可以使用');
    }else{
        showFeedback('error', '非常遗憾，您的用户名太受欢迎，请重新选一个');
    }
}).catch(
	error => {
        showFeedback('error', `${error.code}: ${error.message}`);
    }
)
```

3、发送 post 请求

3.1 以表单方式提交数据

```javascript
axios.post('check', `username=${username}`, {
    headers: {
        // 对于axios来说，这不是必须的，可以省略。它可以根据第二个参数name=value自动决定采用表单方式提交数据
        'content-type': 'application/x-www-form-urlencoded'
    }
}).then(reponse => {
    if(reponse.data.success) {
        showFeedback('success', '恭喜你，您的用户名可以使用')
    }esle{
        showFeedback('error', '非常遗憾，您的用户名太受欢迎，请重新选一个')
    }
}).catch(error => {
    showFeedback('error', `${error.code}: ${error.message}`);
})
```

3.2 以 json 方式提交数据

```javascript
axios.post('check', {username: username}, {
    headers: {
        // 对于axios来说，这不是必须的，可以省略。它可以根据第二个参数是js对象，会自动决定采用json方式提交数据
        'content-type': 'application/json'
    }
}).then(reponse => {
    if(reponse.data.success) {
        showFeedback('success', '恭喜你，您的用户名可以使用')
    }esle{
        showFeedback('error', '非常遗憾，您的用户名太受欢迎，请重新选一个')
    }
}).catch(error => {
    showFeedback('error', `${error.code}: ${error.message}`);
})
```

3.3 提交图片数据

```javascript
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>文件上传</title>
    <script src="https://cdn.bootcdn.net/ajax/libs/axios/1.6.7/axios.min.js"></script>
</head>
<body>
    <input type="file" id="file-upload">
    <button id="upload-btn">上传</button>
    <div id="feedback"></div>

    <script>
        document.getElementById('upload-btn').addEventListener('click', function() {
            const file = document.getElementById('file-upload').files[0]; // files[0] 取第一个文件，file输入框可以一次选多个文件 
            if (!file) return;
            
            const formData = new FormData();
            formData.append('file', file);
            
            axios.post('/upload', formData, {
                headers: {
                    'Content-Type': 'multipart/form-data'
                }
            })
            .then(function(response) {
                document.getElementById('feedback').textContent = '上传成功'；
            })
            .catch(function(error) {
                document.getElementById('feedback').textContent = '上传失败'；
            })
    	})
    </script>
</body>
</html>
```

3.4 Axios + async/await

终极建议方案：

```javascript

async function checkUsername(username) {
    const params = new URLSearchParams();
    params.append('username', username);
}

try {
    const response = await axios.post('check', params);
    if (response.data.success) {
        showFeedback('success', '恭喜你，您的用户名可以使用')
    } esle {
        showFeedback('error', '非常遗憾，您的用户名太受欢迎，请重新选一个')
    }
} catch(error) {
    showFeedback('error', `${error.code}: ${error.message}`);
}

checkUsername(username);
```





