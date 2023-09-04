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
        <div class="split-container">
            <div id="split_texts">
                <!-- Split texts will be displayed here -->
            </div>
            <div id="displayed_text">
                <!-- Clicked text will be displayed here -->
            </div>
        </div>
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
                .then(response => response.json())  // Parse the response as JSON
                .then(data => {
                    // Handle the split data and activate the second tab
                    displaySplitTexts(data);
                    openTab(event, 'tab2');
                })
                .catch(error => {
                    console.error('Error:', error);
                });

            // Prevent form submission (which would reload the page)
            return false;
        }

        function displaySplitTexts(data) {
            var splitTextsDiv = document.getElementById("split_texts");
            splitTextsDiv.innerHTML = "";  // Clear the previous content
            data.forEach(function (text) {
                var div = document.createElement("div");
                div.className = "clickable-div";
                div.textContent = text;
                div.onmouseover = function () { makeClickable(this); };
                div.onclick = function () { displayText(this); };
                splitTextsDiv.appendChild(div);
            });
        }

        function displayText(element) {
            var displayedText = document.getElementById("displayed_text");
            displayedText.innerHTML = element.textContent;
        }
        function makeClickable(element) {
            element.style.cursor = "pointer";
        }

        // Initially, show the first tab
        document.getElementById("tab1").style.display = "block";
    </script>
</body>

</html>



```
