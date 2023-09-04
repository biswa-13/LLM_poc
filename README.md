# LLM_poc

```html


<!DOCTYPE html>
<html>
<head>
<link rel="stylesheet" type="text/css" href="{{ url_for('static', filename='styles.css') }}">
    <title>X12 Data Splitter</title>
</head>
<body>
    <h1>X12 Data Splitter</h1>

    <!-- Tabs -->
    <div id="tabs">
        <button class="tablink" onclick="openTab(event, 'tab1')">Tab 1</button>
        <button class="tablink" onclick="openTab(event, 'tab2')">Tab 2</button>
    </div>

    <!-- Tab 1: Input X12 Data -->
    <div id="tab1" class="tabcontent">
        <textarea id="x12_data" rows="5" cols="50" placeholder="Enter X12 Claim Data"></textarea>
        <button onclick="splitAndActivateTab()">Next</button>
    </div>

    <!-- Tab 2: Display Split Texts -->
    <div id="tab2" class="tabcontent">
        <div id="split_texts">
            {% for text in split_texts %}
                <div class="clickable-div" onmouseover="makeClickable(this)" onclick="displayText(this)">{{ text }}</div>
            {% endfor %}
        </div>
        <div id="displayed_text"></div>
    </div>

    <!-- JavaScript for Tabs and Interaction -->
    <script>
        function openTab(evt, tabName) {
            var i, tabcontent, tablinks;
            tabcontent = document.getElementsByClassName("tabcontent");
            for (i = 0; i < tabcontent.length; i++) {
                tabcontent[i].style.display = "none";
            }
            tablinks = document.getElementsByClassName("tablink");
            for (i = 0; i < tablinks.length; i++) {
                tablinks[i].className = tablinks[i].className.replace(" active", "");
            }
            document.getElementById(tabName).style.display = "block";
            evt.currentTarget.className += " active";
        }

        function splitAndActivateTab() {
            var x12Data = document.getElementById("x12_data").value;
            fetch('/split', {
                method: 'POST',
                body: new URLSearchParams({ 'x12_data': x12Data }),
                headers: {
                    'Content-Type': 'application/x-www-form-urlencoded',
                },
            })
            .then(response => response.text())
            .then(data => {
                // Pass the event and tab name to the openTab function
                openTab({ currentTarget: document.querySelector(".tablink:nth-child(2)") }, 'tab2');
            })
            .catch(error => {
                console.error('Error:', error);
            });
        
            // Prevent form submission (which would reload the page)
            return false;
        }


        function makeClickable(element) {
            element.style.cursor = "pointer";
        }

        function displayText(element) {
            var displayedText = document.getElementById("displayed_text");
            displayedText.innerHTML = element.textContent;
        }

        // Initially, show the first tab
        document.getElementById("tab1").style.display = "block";
    </script>
</body>
</html>


```


```css



/* styles.css */

/* Body styling */
body {
    font-family: Arial, sans-serif;
    background-color: #f2f2f2;
    margin: 0;
    padding: 0;
}

/* Header styling */
h1 {
    text-align: center;
    padding: 20px;
    background-color: #333;
    color: #fff;
}

/* Tabs styling */
#tabs {
    text-align: center;
    margin-top: 20px;
}

.tablink {
    background-color: #333;
    color: #fff;
    border: none;
    padding: 10px 20px;
    cursor: pointer;
    font-size: 16px;
    margin: 5px;
}

.tablink:hover {
    background-color: #555;
}

/* Tab content styling */
.tabcontent {
    display: none;
    padding: 20px;
    background-color: #fff;
    border: 1px solid #ccc;
}

/* Textarea and button styling */
#x12_data {
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
}

button {
    padding: 10px 20px;
    background-color: #333;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background-color: #555;
}

/* Split text div styling */
.clickable-div {
    cursor: pointer;
    padding: 5px;
    margin: 5px;
    background-color: #f2f2f2;
    border: 1px solid #ccc;
    border-radius: 5px;
}

.clickable-div:hover {
    background-color: #ddd;
}

/* Displayed text div styling */
#displayed_text {
    margin-top: 20px;
    font-weight: bold;
}



```
