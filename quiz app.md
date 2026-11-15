
import 'package:flutter/material.dart';

void main() {
  runApp(const MaterialApp(
    debugShowCheckedModeBanner: false,
    home: QuizPage(),
  ));
}

// Question model
class Question {
  final String text;
  final List<String> options;
  final int answerIndex;

  const Question(
    this.text,
    this.options,
    this.answerIndex,
  );
}

// Ten quiz questions
const List<Question> questions = [
  Question('Which language does Flutter use?',
      ['Java', 'Dart', 'Python', 'C++'], 1),

  Question('Everything in Flutter is a?',
      ['Widget', 'Activity', 'Class', 'File'], 0),

  Question('Which function starts a Flutter app?',
      ['start()', 'main()', 'run()', 'open()'], 1),

  Question('Which widget displays text?',
      ['Image', 'Text', 'Button', 'Row'], 1),

  Question('Which widget creates a vertical layout?',
      ['Row', 'Column', 'Stack', 'Center'], 1),

  Question('Which widget creates a button?',
      ['ElevatedButton', 'Text', 'Container', 'Icon'], 0),

  Question('Which widget displays an image?',
      ['Text', 'Image', 'Row', 'Scaffold'], 1),

  Question('Which function updates state?',
      ['print()', 'setState()', 'main()', 'build()'], 1),

  Question('Which widget provides an app structure?',
      ['Scaffold', 'Text', 'Icon', 'Padding'], 0),

  Question('Which file contains Flutter dependencies?',
      ['main.dart', 'pubspec.yaml', 'index.html', 'style.css'], 1),
];

class QuizPage extends StatefulWidget {
  const QuizPage({super.key});

  @override
  State<QuizPage> createState() => _QuizPageState();
}

class _QuizPageState extends State<QuizPage> {
  int index = 0;
  int score = 0;
  int? selected;

  // Select answer
  void chooseAnswer(int option) {
    if (selected != null) return;

    setState(() {
      selected = option;

      if (option == questions[index].answerIndex) {
        score++;
      }
    });
  }

  // Move to next question
  void nextQuestion() {
    setState(() {
      index++;
      selected = null;
    });
  }

  // Restart quiz
  void restartQuiz() {
    setState(() {
      index = 0;
      score = 0;
      selected = null;
    });
  }

  @override
  Widget build(BuildContext context) {
    // Results screen
    if (index >= questions.length) {
      return Scaffold(
        appBar: AppBar(
          title: const Text('Quiz Results'),
          backgroundColor: Colors.blue,
        ),
        body: Center(
          child: Column(
            mainAxisSize: MainAxisSize.min,
            children: [
              const Icon(
                Icons.emoji_events,
                size: 80,
                color: Colors.orange,
              ),
              const Text(
                'Quiz Completed!',
                style: TextStyle(fontSize: 25),
              ),
              const SizedBox(height: 20),
              Text(
                'Your Score: $score / ${questions.length}',
                style: const TextStyle(
                  fontSize: 28,
                  fontWeight: FontWeight.bold,
                ),
              ),
              const SizedBox(height: 20),
              ElevatedButton(
                onPressed: restartQuiz,
                child: const Text('Try Again'),
              ),
            ],
          ),
        ),
      );
    }

    final question = questions[index];

    return Scaffold(
      appBar: AppBar(
        title: const Text('Flutter Quiz App'),
        backgroundColor: Colors.blue,
      ),

      body: Padding(
        padding: const EdgeInsets.all(16),

        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            Text(
              'Question ${index + 1} of ${questions.length}',
              style: const TextStyle(fontSize: 18),
            ),

            const SizedBox(height: 15),

            LinearProgressIndicator(
              value: (index + 1) / questions.length,
            ),

            const SizedBox(height: 30),

            Text(
              question.text,
              style: const TextStyle(
                fontSize: 22,
                fontWeight: FontWeight.bold,
              ),
            ),

            const SizedBox(height: 20),

            // Answer options
            for (int i = 0; i < question.options.length; i++)
              Card(
                color: selected == null
                    ? null
                    : i == question.answerIndex
                        ? Colors.green.shade200
                        : i == selected
                            ? Colors.red.shade200
                            : null,

                child: ListTile(
                  title: Text(question.options[i]),
                  onTap: selected == null
                      ? () => chooseAnswer(i)
                      : null,
                ),
              ),

            const Spacer(),

            if (selected != null)
              ElevatedButton(
                onPressed: nextQuestion,
                child: Text(
                  index == questions.length - 1
                      ? 'Show Results'
                      : 'Next Question',
                ),
              ),
          ],
        ),
      ),
    );
  }
}
