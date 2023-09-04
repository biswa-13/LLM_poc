# LLM_poc

```html


<!DOCTYPE html>
<html>
<head>
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
                openTab(event, 'tab2');
            });
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
