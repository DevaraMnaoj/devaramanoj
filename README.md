from flask import Flask
import os
from datetime import datetime
import subprocess

app = Flask(__name__)

@app.route("/htop")
def htop():
    # Get the system username
    username = os.getlogin()

    # Get the current server time in IST
    server_time = datetime.now().astimezone().strftime('%Y-%m-%d %H:%M:%S')

    # Get the output of the `top` command
    top_output = subprocess.check_output("top -n 1", shell=True).decode()

    # Replace 'Your Name' with your full name
    return f"""
    <h1>System
