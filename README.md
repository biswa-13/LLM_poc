# LLM_poc

```html
<!DOCTYPE html>
<html>
<head>
    <title>Server Time</title>
</head>
<body>
    <h1>Get Server Time</h1>
    <p>Click the button to get the server time:</p>
    <button id="get-time-button">Get Time</button>
    <p>Server Time: <span id="server-time"></span></p>

    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script>
        $(document).ready(function () {
            $('#get-time-button').click(function () {
                $.ajax({
                    type: 'POST',
                    url: '/get_server_time',
                    success: function (response) {
                        $('#server-time').text(response.server_time);
                    }
                });
            });
        });
    </script>
</body>
</html>


```
