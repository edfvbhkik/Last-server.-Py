
from flask import Flask, request, jsonify, send_from_directory
from openai import OpenAI
import os

app = Flask(__name__)
client = OpenAI()

@app.route("/")
def home():
    return send_from_directory(".", "index.html")

@app.route("/chat", methods=["POST"])
def chat():
    try:
        message = request.json["message"]

        response = client.responses.create(
            model="gpt-5-mini",
            instructions=(
                "You are NAYMO AI MINI, a friendly, funny AI. "
                "You care about the environment. "
                "Give practical advice about reducing environmental harm. "
                "Never claim AI has zero environmental impact."
            ),
            input=message
        )

        return jsonify({"reply": response.output_text})

    except Exception as error:
        return jsonify({"error": "Naymo had a problem. Try again later."}), 500

if __name__ == "__main__":
    app.run(host="127.0.0.1", port=5000)
