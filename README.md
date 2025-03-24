from flask import Flask
import os
import datetime
import subprocess
import getpass
import pytz  # Install via 'pip install pytz'

app = Flask(__name__)

@app.route('/htop')
def htop():
    name = "Your Full Name"  # Replace with your actual name
    username = getpass.getuser()  # More reliable way to get the system username

    # Get server time in IST
    ist = pytz.timezone('Asia/Kolkata')
    server_time = datetime.datetime.now(ist).strftime("%Y-%m-%d %H:%M:%S IST")

    # Get system processes (Windows uses 'tasklist', Linux uses 'top')
    try:
        if os.name == "nt":  # Windows
            top_output = subprocess.check_output("tasklist | findstr /B /C:\"Image Name\"", shell=True).decode("utf-8")
        else:  # Linux/Mac
            top_output = subprocess.check_output("top -bn1 | head -10", shell=True).decode("utf-8")
    except Exception as e:
        top_output = f"Error fetching process list: {str(e)}"

    return f"""
    <h1>Server Info</h1>
    <p><b>Name:</b> {name}</p>
    <p><b>Username:</b> {username}</p>
    <p><b>Server Time (IST):</b> {server_time}</p>
    <pre>{top_output}</pre>
    """

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8000)
