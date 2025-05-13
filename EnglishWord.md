# 플러터로 영어 단어 암기 어플 만들기
## main.dart 
<pre>
  <code>
    import 'package:flutter/material.dart';
import 'quiz_level_screen.dart'; // 퀴즈 난이도 화면 import

void main() {
  runApp(const VocabularyApp());
}

class VocabularyApp extends StatelessWidget {
  const VocabularyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      home: MainMenuScreen(),
      debugShowCheckedModeBanner: false,
    );
  }
}

class MainMenuScreen extends StatelessWidget {
  const MainMenuScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Container(
        width: double.infinity,
        padding: const EdgeInsets.all(24),
        decoration: const BoxDecoration(
          gradient: LinearGradient(
            colors: [Colors.blue, Color(0xFFB2A4FF)],
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
          ),
        ),
        child: Column(
          children: [
            const SizedBox(height: 100),
            const Text(
              'Vucabluary', // 오타 그대로 유지
              style: TextStyle(
                fontSize: 32,
                fontWeight: FontWeight.bold,
                color: Colors.white,
              ),
            ),
            const Spacer(),
            Wrap(
              spacing: 20,
              runSpacing: 20,
              alignment: WrapAlignment.center,
              children: [
                _buildMenuButton(context, 'Quiz'),
                _buildMenuButton(context, '내 정보'),
                _buildMenuButton(context, '설정'),
                _buildMenuButton(context, '종료'),
              ],
            ),
            const SizedBox(height: 60),
          ],
        ),
      ),
    );
  }

  Widget _buildMenuButton(BuildContext context, String label) {
    return SizedBox(
      width: 160,
      height: 80,
      child: ElevatedButton(
        onPressed: () {
          if (label == 'Quiz') {
            Navigator.push(
              context,
              MaterialPageRoute(
                builder: (context) => const QuizLevelScreen(),
              ),
            );
          } else {
            // 다른 버튼에 대한 동작 처리
          }
        },
        style: ElevatedButton.styleFrom(
          backgroundColor: Colors.white,
          foregroundColor: Colors.deepPurple,
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(16),
          ),
          elevation: 6,
        ),
        child: Text(
          label,
          style: const TextStyle(
            fontSize: 20,
            fontWeight: FontWeight.bold,
          ),
        ),
      ),
    );
  }
}
</code>
</pre>
