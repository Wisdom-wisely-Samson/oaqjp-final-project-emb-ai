"""
Flask server for emotion detection application.
Provides endpoints for rendering the frontend and detecting emotions.
"""

from flask import Flask, request, render_template, jsonify
from EmotionDetection.emotion_detection import emotion_detector  # import your function

app = Flask(__name__)

@app.route("/")
def index():
    """
    Render the index.html page from the templates folder.
    """
    return render_template("index.html")

@app.route("/emotionDetector", methods=["GET", "POST"])
def detect_emotion():
    """
    Detect emotions from a given statement.
    Supports both GET (query string) and POST (JSON body).
    Returns emotion scores and a formatted response string.
    """
    if request.method == "GET":
        statement = request.args.get("textToAnalyze", "")
    else:  # POST
        data = request.get_json()
        if not data or "text" not in data:
            return jsonify({"error": "Missing 'text' in request body"}), 400
        statement = data["text"]

    result = emotion_detector(statement)

    if result["dominant_emotion"] is None:
        response_text = (
            "For the given statement, no emotions were detected "
            "because the input was blank."
        )
    else:
        response_text = (
            f"For the given statement, the system response is "
            f"'anger': {result['anger']}, "
            f"'disgust': {result['disgust']}, "
            f"'fear': {result['fear']}, "
            f"'joy': {result['joy']} and "
            f"'sadness': {result['sadness']}. "
            f"The dominant emotion is {result['dominant_emotion']}."
        )

    return jsonify({
        "anger": result["anger"],
        "disgust": result["disgust"],
        "fear": result["fear"],
        "joy": result["joy"],
        "sadness": result["sadness"],
        "dominant_emotion": result["dominant_emotion"],
        "response": response_text
    })

if __name__ == "__main__":
    app.run(host="localhost", port=5000, debug=True)
