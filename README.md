# PimoroniBlinktFromTheBrowser

Control Pimoroni Blinkt from your web browser.

# Step 1: Install Flask

If you haven’t already installed Flask on your Raspberry Pi, open a terminal and run:

pip3 install Flask

# Step 2: Create a Flask Application

Create a new directory for your project and navigate into it:

mkdir blinkt_web_control
cd blinkt_web_control

Create a Python file for your app, let's call it app.py:

touch app.py

Open app.py in a text editor and write the initial Flask application code:

from flask import Flask, render_template, request, redirect, url_for
app = Flask(__name__)

@app.route('/')
def index():
    return 'Hello, Blinkt! Web Control!'

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')

This code creates a basic Flask app that will display "Hello, Blinkt! Web Control!" at the homepage.

# Step 3: Run Your Flask Application

In the terminal, run:

sh
Copy code
python3 app.py

Your Flask server should start, and you should be able to access it via http://<RaspberryPi_IP>:5000 on any browser within your network. Replace <RaspberryPi_IP> with your Raspberry Pi's IP address.


**Step 4: Create the Web Interface**
Stop your Flask app by pressing CTRL+C in the terminal.
Create a templates directory in your project folder:

mkdir templates

Create an HTML file for your web interface. Let’s call it index.html. Place it inside the templates folder.

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Blinkt! Controller</title>
</head>
<body>
<h1>Blinkt! Controller</h1>
<form action="/run-script" method="post">
<select name="script">
<option value="example1.py">Example 1</option>
<option value="example2.py">Example 2</option>
<!-- Add more options based on your example scripts -->
</select>
<button type="submit">Go</button>
</form>
</body>
</html>

Modify app.py to render this template:

from flask import Flask, render_template, request, redirect, url_for
app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
    
# Step 5: Handling Script Execution

To run scripts from Flask, you’ll use the subprocess module. Update app.py to include a new route for running scripts:

import subprocess
from flask import Flask, render_template, request, redirect, url_for

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/run-script', methods=['POST'])
def run_script():
    script_name = request.form['script']
    # Replace the paths below with the actual paths to your scripts
    subprocess.Popen(['python3', f'/path/to/blinkt/examples/{script_name}'])
    return redirect(url_for('index'))

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
    
Replace /path/to/blinkt/examples/ with the actual directory path of your Blinkt! example scripts.

# Step 6: Running Your Application

Run your Flask app again with python3 app.py. Now, when you visit your Raspberry Pi's IP address at port 5000, you should see your web interface with a dropdown list of example scripts. Selecting one and clicking “Go” will run the script on your Raspberry Pi, controlling the Blinkt! LEDs.

Note on Security
Running scripts from a web interface can be risky, especially if the device is accessible on a public network. Make sure your Raspberry Pi is secure and consider implementing authentication for the web interface.
That's a basic setup to get you started. You can expand this by adding more features, like stopping scripts or creating custom lighting controls directly from the web interface. Enjoy your project, and light up your world with Blinkt!
