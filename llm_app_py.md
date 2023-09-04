```python

from flask import Flask, render_template, request, jsonify

app = Flask(__name__)

split_texts = []

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/split', methods=['POST'])
def split():
    global split_texts
    x12_data = request.form['x12_data']
    split_texts = x12_data.split('~')

    # Pass the split data as JSON
    return jsonify(split_texts)

@app.route('/uppercase', methods=['POST'])
def uppercase():
    text = request.form['text']
    # Convert text to uppercase
    uppercase_text = text.upper()
    return jsonify({'uppercase_text': uppercase_text})

if __name__ == '__main__':
    app.run(debug=True)


```
