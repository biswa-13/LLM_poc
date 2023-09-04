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
            <div id="displayed_text" class="loading">Loading...</div> <!-- Add class "loading" -->
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
                .then(response => response.json())
                .then(data => {
                    // Pass the event and tab name to the openTab function
                    openTab({ currentTarget: document.querySelector(".tablink:nth-child(2)") }, 'tab2');
                    // Display the split data in the second tab
                    displaySplitTexts(data);
                })
                .catch(error => {
                    console.error('Error:', error);
                });

            // Prevent form submission (which would reload the page)
            return false;
        }

        function displayText(element) {
            // Disable the entire page
            document.body.style.pointerEvents = "none";

            // Display "Loading..." message in the second div
            var displayedText = document.getElementById("displayed_text");
            displayedText.innerHTML = "Loading...";

            var text = element.textContent;
            fetch('/uppercase', {
                method: 'POST',
                body: new URLSearchParams({ 'text': text }),
                headers: {
                    'Content-Type': 'application/x-www-form-urlencoded',
                },
            })
                .then(response => response.json())
                .then(data => {
                    // Enable the entire page
                    document.body.style.pointerEvents = "auto";

                    // Display the uppercase text in the second div
                    displayedText.innerHTML = data.uppercase_text;
                })
                .catch(error => {
                    console.error('Error:', error);

                    // Enable the entire page (in case of an error)
                    document.body.style.pointerEvents = "auto";
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
