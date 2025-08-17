import os
from flask import Flask, jsonify
import requests
from datetime import datetime

app = Flask(__name__)

REFRESH_TOKEN = "1//0eYCMd2e_zRE6CgYIARAAGA4SNwF-L9IrkKmsOtImi93IZRL7FR6sx-bfeUZa0IgCsCIjnR36oc4AtzgGvgAtlsYEv5jNqO0vd7U"
CLIENT_ID = "9460567014-ptq4gh2223kdkplv6345q8j0gns1llua.apps.googleusercontent.com"
CLIENT_SECRET = "ys6cor_wYtysFVJ3nqAIKjvH"

@app.route('/')
def home():
    return jsonify({"status": "Running"})

@app.route('/ClientDown.dat')
def get_token():
    url = "https://oauth2.googleapis.com/token"
    data = {
        "client_id": CLIENT_ID,
        "client_secret": CLIENT_SECRET,
        "refresh_token": REFRESH_TOKEN,
        "grant_type": "refresh_token"
    }
    response = requests.post(url, data=data)
    token_data = response.json()
    
    result = {
        "access_token": token_data.get("access_token", ""),
        "refresh_token": REFRESH_TOKEN,
        "expires_in": 3599
    }
    return jsonify(result)

if __name__ == '__main__':
    port = int(os.environ.get('PORT', 8080))
    app.run(host='0.0.0.0', port=port)
