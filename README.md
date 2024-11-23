from flask import Flask, request, jsonify
import openai

app = Flask(__name__)

# Configure your OpenAI API key
openai.api_key = "your_openai_api_key"

@app.route('/')
def home():
    return "Welcome to the Chatbot API! Send questions to /ask endpoint."

@app.route('/ask', methods=['POST'])
def ask_question():
    # Parse the user input
    user_input = request.json.get('question', '')
    
    if not user_input:
        return jsonify({"error": "Please provide a question."}), 400

    try:
        # Get a response from OpenAI's model
        response = openai.Completion.create(
            engine="text-davinci-003",
            prompt=user_input,
            max_tokens=150
        )

        answer = response['choices'][0]['text'].strip()
        return jsonify({"answer": answer})
    
    except Exception as e:
        return jsonify({"error": str(e)}), 500

if __name__ == '__main__':
    app.run(debug=True)

