1. 플러터로 숫자 야구 게임 만들기 ver2.
이미지를 넣고 진짜 게임인것 처럼 만들어 보았다.
ver 3에는 소리도 들어갈듯
내가 원하는 대로 만들어주는게 재밌어서 새벽 5시 까지 해버렸다..

(1) main.dart
<pre>
<code>
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import 'dart:io';
import 'package:google_fonts/google_fonts.dart'; // Google Fonts 사용
import 'three_digit_game.dart';
import 'four_digit_game.dart';

void main() {
  runApp(NumberBaseballApp());
}

class NumberBaseballApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: '숫자 야구 게임',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: MainMenu(),
      debugShowCheckedModeBanner: false,
    );
  }
}

class MainMenu extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    double screenWidth = MediaQuery.of(context).size.width;
    double screenHeight = MediaQuery.of(context).size.height;

    return Scaffold(
      body: Container(
        decoration: BoxDecoration(
          image: DecorationImage(
            image: AssetImage('assets/background.jpg'), // 배경 이미지 추가
            fit: BoxFit.cover,
          ),
        ),
        child: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              // "Guess Base" 제목
              Row(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Stack(
                    children: [
                      // 검은색 테두리 텍스트
                      Text(
                        'Guess',
                        style: GoogleFonts.rubik(
                          fontSize: screenWidth * 0.12,
                          fontWeight: FontWeight.bold,
                          foreground: Paint()
                            ..style = PaintingStyle.stroke
                            ..strokeWidth = 6 // 테두리 더 두껍게 설정
                            ..color = Colors.black,
                        ),
                      ),
                      // 원래 색상 텍스트
                      Text(
                        'Guess',
                        style: GoogleFonts.rubik(
                          fontSize: screenWidth * 0.12,
                          fontWeight: FontWeight.bold,
                          color: Colors.lightGreen, // 원래 색상 유지
                        ),
                      ),
                    ],
                  ),
                  SizedBox(width: 10),
                  Stack(
                    children: [
                      // 검은색 테두리 텍스트
                      Text(
                        'Base',
                        style: GoogleFonts.rubik(
                          fontSize: screenWidth * 0.12,
                          fontWeight: FontWeight.bold,
                          foreground: Paint()
                            ..style = PaintingStyle.stroke
                            ..strokeWidth = 6 // 테두리 더 두껍게 설정
                            ..color = Colors.black,
                        ),
                      ),
                      // 원래 색상 텍스트
                      Text(
                        'Base',
                        style: GoogleFonts.rubik(
                          fontSize: screenWidth * 0.12,
                          fontWeight: FontWeight.bold,
                          color: Colors.orange, // 원래 색상 유지
                        ),
                      ),
                    ],
                  ),
                ],
              ),
              SizedBox(height: screenHeight * 0.05),

              // "난이도(하)" 버튼
              buildFlatButton(
                screenWidth: screenWidth,
                screenHeight: screenHeight,
                label: '난이도(하)',
                color: Colors.green,
                font: GoogleFonts.rubik(
                  fontSize: screenWidth * 0.06,
                  fontWeight: FontWeight.bold,
                  color: Colors.white,
                ),
                onPressed: () {
                  Navigator.push(
                    context,
                    MaterialPageRoute(builder: (context) => ThreeDigitGame()),
                  );
                },
                borderColor: Colors.black,
                borderWidth: 3.0,
              ),
              SizedBox(height: screenHeight * 0.03), // 버튼 간격을 줄임

              // "난이도(중)" 버튼
              buildFlatButton(
                screenWidth: screenWidth,
                screenHeight: screenHeight,
                label: '난이도(중)',
                color: Colors.orange, // 주황색으로 변경
                font: GoogleFonts.rubik(
                  fontSize: screenWidth * 0.06,
                  fontWeight: FontWeight.bold,
                  color: Colors.white,
                ),
                onPressed: () {
                  Navigator.push(
                    context,
                    MaterialPageRoute(builder: (context) => FourDigitGame()),
                  );
                },
                borderColor: Colors.black,
                borderWidth: 3.0,
              ),
              SizedBox(height: screenHeight * 0.03), // 버튼 간격을 줄임

              // "설정" 버튼
              buildFlatButton(
                screenWidth: screenWidth,
                screenHeight: screenHeight,
                label: '설정',
                color: Colors.blue,
                font: GoogleFonts.rubik(
                  fontSize: screenWidth * 0.06,
                  fontWeight: FontWeight.bold,
                  color: Colors.white,
                ),
                onPressed: () {
                  showDialog(
                    context: context,
                    builder: (BuildContext context) {
                      return AlertDialog(
                        title: Text('설정'),
                        content: Text('설정 화면은 준비 중입니다.'),
                        actions: [
                          TextButton(
                            onPressed: () {
                              Navigator.of(context).pop();
                            },
                            child: Text('닫기'),
                          ),
                        ],
                      );
                    },
                  );
                },
                borderColor: Colors.black,
                borderWidth: 3.0,
              ),
              SizedBox(height: screenHeight * 0.03), // 버튼 간격을 줄임

              // "게임 종료" 버튼
              buildFlatButton(
                screenWidth: screenWidth,
                screenHeight: screenHeight,
                label: '게임 종료',
                color: Colors.red,
                font: GoogleFonts.rubik(
                  fontSize: screenWidth * 0.06,
                  fontWeight: FontWeight.bold,
                  color: Colors.white,
                ),
                onPressed: () {
                  showDialog(
                    context: context,
                    builder: (BuildContext context) {
                      return AlertDialog(
                        title: Text('게임 종료'),
                        content: Text('게임을 종료하시겠습니까?'),
                        actions: [
                          TextButton(
                            onPressed: () {
                              Navigator.of(context).pop();
                            },
                            child: Text('취소'),
                          ),
                          TextButton(
                            onPressed: () {
                              if (Platform.isAndroid) {
                                SystemNavigator.pop();
                              } else if (Platform.isIOS) {
                                exit(0);
                              }
                            },
                            child: Text('확인'),
                          ),
                        ],
                      );
                    },
                  );
                },
                borderColor: Colors.black,
                borderWidth: 3.0,
              ),
            ],
          ),
        ),
      ),
    );
  }

  Widget buildFlatButton({
    required double screenWidth,
    required double screenHeight,
    required String label,
    required Color color,
    required TextStyle font,
    required VoidCallback onPressed,
    required Color borderColor,
    required double borderWidth,
  }) {
    return ElevatedButton(
      onPressed: onPressed,
      style: ElevatedButton.styleFrom(
        elevation: 8,
        padding: EdgeInsets.zero,
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(20),
          side: BorderSide(color: borderColor, width: borderWidth),
        ),
        backgroundColor: color,
      ),
      child: Container(
        width: screenWidth * 0.7,
        height: screenHeight * 0.1,
        alignment: Alignment.center,
        child: Text(
          label,
          style: font,
        ),
      ),
    );
  }
}

</code>
</pre>

(2) three_digit_game.dart
<pre>
<code>
import 'dart:math';
import 'package:flutter/material.dart';

class ThreeDigitGame extends StatefulWidget {
  @override
  _ThreeDigitGameState createState() => _ThreeDigitGameState();
}

class _ThreeDigitGameState extends State<ThreeDigitGame> {
  final TextEditingController _controller = TextEditingController();
  final List<int> _randomNumbers = [];
  final List<String> _guessHistory = [];
  String _resultMessage = '';
  int _attempts = 0;
  int _remainingChances = 10;

  @override
  void initState() {
    super.initState();
    _generateRandomNumbers();
  }

  void _generateRandomNumbers() {
    final random = Random();
    _randomNumbers.clear();
    while (_randomNumbers.length < 3) {
      int num = random.nextInt(10);
      if (!_randomNumbers.contains(num)) {
        _randomNumbers.add(num);
      }
    }
    print('정답 숫자(디버깅용): $_randomNumbers');
  }

  void _checkGuess() {
    if (_remainingChances <= 0) return;

    String input = _controller.text;

    if (input.length != 3 || input.contains(RegExp(r'[^0-9]'))) {
      setState(() {
        _resultMessage = '3자리 숫자를 입력하세요.';
      });
      return;
    }

    if (input.split('').toSet().length != input.length) {
      setState(() {
        _resultMessage = '중복된 숫자입니다.';
      });
      return;
    }

    final List<int> guess = input.split('').map(int.parse).toList();
    int strikes = 0;
    int balls = 0;

    for (int i = 0; i < 3; i++) {
      if (guess[i] == _randomNumbers[i]) {
        strikes++;
      } else if (_randomNumbers.contains(guess[i])) {
        balls++;
      }
    }

    setState(() {
      _attempts++;
      _remainingChances--;
      _guessHistory.add(input);

      if (strikes == 3) {
        _resultMessage = '축하합니다! 정답입니다. 시도 횟수: $_attempts번';
      } else if (_remainingChances == 0) {
        _resultMessage = '탈락했습니다! 정답은 $_randomNumbers입니다.';
      } else {
        _resultMessage = '스트라이크: $strikes, 볼: $balls';
      }

      _controller.clear();
    });
  }

  void _clearInput() {
    setState(() {
      if (_controller.text.isNotEmpty) {
        _controller.text =
            _controller.text.substring(0, _controller.text.length - 1);
      }
    });
  }

  void _resetGame() {
    setState(() {
      _controller.clear();
      _resultMessage = '';
      _attempts = 0;
      _remainingChances = 10;
      _guessHistory.clear();
      _generateRandomNumbers();
    });
  }

  @override
  Widget build(BuildContext context) {
    double screenWidth = MediaQuery.of(context).size.width;
    double screenHeight = MediaQuery.of(context).size.height;

    return Scaffold(
      body: Container(
        decoration: BoxDecoration(
          image: DecorationImage(
            image: AssetImage('assets/background2.jpg'),
            fit: BoxFit.cover,
          ),
        ),
        child: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            children: [
              SizedBox(height: screenHeight * 0.03),
              Row(
                mainAxisAlignment: MainAxisAlignment.spaceBetween,
                children: [
                  Row(
                    children: [
                      IconButton(
                        onPressed: () {
                          Navigator.pop(context);
                        },
                        icon: Icon(Icons.arrow_back, color: Colors.black),
                      ),
                      Text(
                        '남은 기회: $_remainingChances번',
                        style: TextStyle(
                          fontSize: screenWidth * 0.05,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                    ],
                  ),
                  ElevatedButton(
                    onPressed: _resetGame,
                    style: ElevatedButton.styleFrom(
                      backgroundColor: Colors.yellow,
                    ),
                    child: Text(
                      '게임 리셋',
                      style: TextStyle(
                        fontWeight: FontWeight.bold,
                        color: Colors.black,
                        fontSize: screenWidth * 0.04,
                      ),
                    ),
                  ),
                ],
              ),
              SizedBox(height: screenHeight * 0.0001),
              GridView.builder(
                shrinkWrap: true,
                gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
                  crossAxisCount: 3,
                  crossAxisSpacing: 6,
                  mainAxisSpacing: 6,
                ),
                itemCount: 3,
                itemBuilder: (context, index) {
                  return Container(
                    alignment: Alignment.center,
                    decoration: BoxDecoration(
                      color: Colors.white.withOpacity(0.7),
                      borderRadius: BorderRadius.circular(8),
                      boxShadow: [
                        BoxShadow(
                          color: Colors.black26,
                          offset: Offset(2, 2),
                          blurRadius: 4,
                        ),
                      ],
                    ),
                    child: Text(
                      index < _controller.text.length
                          ? _controller.text[index]
                          : '',
                      style: TextStyle(
                        fontSize: screenWidth * 0.1,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                  );
                },
              ),
              SizedBox(height: screenHeight * 0.01), // 사각형과 텍스트 간격 조정
              Container(
                alignment: Alignment.center,
                padding: EdgeInsets.symmetric(vertical: 8, horizontal: 16),
                decoration: BoxDecoration(
                  color: Colors.white.withOpacity(0.7), // 흰색 사각형
                  borderRadius: BorderRadius.circular(8),
                  boxShadow: [
                    BoxShadow(
                      color: Colors.black26,
                      offset: Offset(2, 2),
                      blurRadius: 4,
                    ),
                  ],
                ),
                child: Text(
                  _resultMessage,
                  style: TextStyle(
                    fontSize: screenWidth * 0.05,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ),
              SizedBox(height: screenHeight * 0.00001), // 기록 리스트 간격 조정
              Expanded(
                child: ListView.builder(
                  itemCount: _guessHistory.length > 9
                      ? 9
                      : _guessHistory.length,
                  itemBuilder: (context, index) {
                    final guess = _guessHistory[index];
                    int strikes = 0;
                    int balls = 0;

                    final List<int> guessNumbers =
                    guess.split('').map(int.parse).toList();

                    for (int i = 0; i < 3; i++) {
                      if (guessNumbers[i] == _randomNumbers[i]) {
                        strikes++;
                      } else if (_randomNumbers.contains(guessNumbers[i])) {
                        balls++;
                      }
                    }

                    return Container(
                      margin: EdgeInsets.symmetric(vertical: 4),
                      padding: EdgeInsets.all(8),
                      decoration: BoxDecoration(
                        color: Colors.blue.shade100,
                        borderRadius: BorderRadius.circular(8),
                        boxShadow: [
                          BoxShadow(
                            color: Colors.black26,
                            offset: Offset(2, 2),
                            blurRadius: 4,
                          ),
                        ],
                      ),
                      child: Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                          Text(
                            '${index + 1}. $guess',
                            style: TextStyle(
                              fontSize: 16,
                              fontWeight: FontWeight.bold,
                            ),
                          ),
                          Text(
                            '스트라이크: $strikes, 볼: $balls',
                            style: TextStyle(
                              fontSize: 16,
                            ),
                          ),
                        ],
                      ),
                    );
                  },
                ),
              ), SizedBox(height: screenHeight * 0.035),

              Column(
                children: [
                  Row(
                    children: ['1', '2', '3', '4', '5'].map((value) {
                      return Expanded(
                          child: _buildKey(value, screenWidth));
                    }).toList(),
                  ),

                  Row(
                    children: ['6', '7', '8', '9', '0'].map((value) {
                      return Expanded(
                          child: _buildKey(value, screenWidth));
                    }).toList(),
                  ),
                  SizedBox(height: screenHeight * 0.02),
                  Row(
                    children: [
                      Expanded(
                        child: _buildActionKey(
                            '확인', Colors.green, _checkGuess, screenWidth),
                      ),
                      SizedBox(width: screenWidth * 0.02),
                      Expanded(
                        child: _buildActionKey(
                            '지우기', Colors.red, _clearInput, screenWidth),
                      ),
                    ],
                  ),
                ],
              ),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildKey(String value, double screenWidth) {
    return GestureDetector(
      onTap: () {
        if (_controller.text.length < 3) {
          setState(() {
            _controller.text += value;
          });
        }
      },

      child: Container(
        margin: EdgeInsets.all(screenWidth * 0.01),
        height: screenWidth * 0.15,
        alignment: Alignment.center,
        decoration: BoxDecoration(
          color: Colors.blue,
          borderRadius: BorderRadius.circular(8),
        ),
        child: Text(
          value,
          style: TextStyle(fontSize: screenWidth * 0.05, color: Colors.white),
        ),
      ),
    );
  }

  Widget _buildActionKey(
      String value, Color color, Function() onTap, double screenWidth) {
    return GestureDetector(
      onTap: onTap,
      child: Container(
        margin: EdgeInsets.all(screenWidth * 0.01),
        height: screenWidth * 0.15,
        alignment: Alignment.center,
        decoration: BoxDecoration(
          color: color,
          borderRadius: BorderRadius.circular(8),
        ),
        child: Text(
          value,
          style: TextStyle(
            fontSize: screenWidth * 0.05,
            color: Colors.white,
          ),
        ),
      ),
    );
  }
}

     </code>
   </pre>

(3) four_digit_game.dart

<pre>
<code>
import 'dart:math';
import 'package:flutter/material.dart';

class FourDigitGame extends StatefulWidget {
  @override
  _FourDigitGameState createState() => _FourDigitGameState();
}

class _FourDigitGameState extends State<FourDigitGame> {
  final TextEditingController _controller = TextEditingController();
  final List<int> _randomNumbers = [];
  final List<String> _guessHistory = []; // 입력 기록 저장
  String _resultMessage = '';
  int _attempts = 0;
  int _remainingChances = 10;

  @override
  void initState() {
    super.initState();
    _generateRandomNumbers();
  }

  void _generateRandomNumbers() {
    final random = Random();
    _randomNumbers.clear();
    while (_randomNumbers.length < 4) { // 4자리 숫자 생성
      int num = random.nextInt(10);
      if (!_randomNumbers.contains(num)) {
        _randomNumbers.add(num);
      }
    }
    print('정답 숫자(디버깅용): $_randomNumbers');
  }

  void _checkGuess() {
    if (_remainingChances <= 0) return;

    String input = _controller.text;

    if (input.length != 4 || input.contains(RegExp(r'[^0-9]'))) { // 4자리 확인
      setState(() {
        _resultMessage = '4자리 숫자를 입력하세요.';
      });
      return;
    }

    if (input.split('').toSet().length != input.length) {
      setState(() {
        _resultMessage = '중복된 숫자입니다.';
      });
      return;
    }

    final List<int> guess = input.split('').map(int.parse).toList();
    int strikes = 0;
    int balls = 0;

    for (int i = 0; i < 4; i++) { // 4자리 비교
      if (guess[i] == _randomNumbers[i]) {
        strikes++;
      } else if (_randomNumbers.contains(guess[i])) {
        balls++;
      }
    }

    setState(() {
      _attempts++;
      _remainingChances--;
      _guessHistory.add(input); // 기록 추가

      if (strikes == 4) {
        _resultMessage = '축하합니다! 정답입니다. 시도 횟수: $_attempts번';
      } else if (_remainingChances == 0) {
        _resultMessage = '탈락했습니다! 정답은 $_randomNumbers입니다.';
      } else {
        _resultMessage = '스트라이크: $strikes, 볼: $balls';
      }

      _controller.clear(); // 텍스트 필드 초기화
    });
  }

  void _clearInput() {
    setState(() {
      if (_controller.text.isNotEmpty) {
        _controller.text = _controller.text.substring(0, _controller.text.length - 1);
      }
    });
  }

  void _resetGame() {
    setState(() {
      _controller.clear();
      _resultMessage = '';
      _attempts = 0;
      _remainingChances = 10;
      _guessHistory.clear();
      _generateRandomNumbers();
    });
  }

  @override
  Widget build(BuildContext context) {
    double screenWidth = MediaQuery.of(context).size.width;
    double screenHeight = MediaQuery.of(context).size.height;

    return Scaffold(
      appBar: AppBar(
        title: Text('난이도(중)'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                Text(
                  '남은 기회: $_remainingChances번',
                  style: TextStyle(
                    fontSize: screenWidth * 0.05,
                    fontWeight: FontWeight.bold,
                  ),
                ),
                ElevatedButton(
                  onPressed: _resetGame,
                  style: ElevatedButton.styleFrom(
                    backgroundColor: Colors.yellow,
                  ),
                  child: Text(
                    '게임 리셋',
                    style: TextStyle(
                      fontWeight: FontWeight.bold,
                      color: Colors.black,
                      fontSize: screenWidth * 0.04,
                    ),
                  ),
                ),
              ],
            ),
            SizedBox(height: screenHeight * 0.02),
            GridView.builder(
              shrinkWrap: true,
              gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
                crossAxisCount: 4, // 정사각형 4개로 변경
                crossAxisSpacing: 6,
                mainAxisSpacing: 6,
              ),
              itemCount: 4,
              itemBuilder: (context, index) {
                return Container(
                  alignment: Alignment.center,
                  decoration: BoxDecoration(
                    color: Colors.white,
                    borderRadius: BorderRadius.circular(8),
                    boxShadow: [
                      BoxShadow(
                        color: Colors.black26,
                        offset: Offset(2, 2),
                        blurRadius: 4,
                      ),
                    ],
                  ),
                  child: Text(
                    index < _controller.text.length ? _controller.text[index] : '',
                    style: TextStyle(
                      fontSize: screenWidth * 0.1,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                );
              },
            ),
            SizedBox(height: screenHeight * 0.02),
            Text(
              _resultMessage,
              style: TextStyle(fontSize: screenWidth * 0.05),
            ),
            SizedBox(height: screenHeight * 0.03),
            Expanded(
              child: ListView.builder(
                itemCount: _guessHistory.length > 9 ? 9 : _guessHistory.length, // 최대 9개의 기록만 표시
                itemBuilder: (context, index) {
                  final guess = _guessHistory[index];
                  int strikes = 0;
                  int balls = 0;

                  final List<int> guessNumbers = guess.split('').map(int.parse).toList();

                  for (int i = 0; i < 4; i++) {
                    if (guessNumbers[i] == _randomNumbers[i]) {
                      strikes++;
                    } else if (_randomNumbers.contains(guessNumbers[i])) {
                      balls++;
                    }
                  }

                  return Container(
                    margin: EdgeInsets.symmetric(vertical: 4),
                    padding: EdgeInsets.all(8),
                    decoration: BoxDecoration(
                      color: Colors.blue.shade100,
                      borderRadius: BorderRadius.circular(8),
                      boxShadow: [
                        BoxShadow(
                          color: Colors.black26,
                          offset: Offset(2, 2),
                          blurRadius: 4,
                        ),
                      ],
                    ),
                    child: Row(
                      mainAxisAlignment: MainAxisAlignment.spaceBetween,
                      children: [
                        Text(
                          '${index + 1}. $guess',
                          style: TextStyle(
                            fontSize: 16,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                        Text(
                          '스트라이크: $strikes, 볼: $balls',
                          style: TextStyle(
                            fontSize: 16,
                          ),
                        ),
                      ],
                    ),
                  );
                },
              ),
            ),
            Column(
              children: [
                Row(
                  children: ['1', '2', '3', '4', '5'].map((value) {
                    return Expanded(child: _buildKey(value, screenWidth));
                  }).toList(),
                ),
                SizedBox(height: screenHeight * 0.02),
                Row(
                  children: ['6', '7', '8', '9', '0'].map((value) {
                    return Expanded(child: _buildKey(value, screenWidth));
                  }).toList(),
                ),
                SizedBox(height: screenHeight * 0.02),
                Row(
                  children: [
                    Expanded(
                      child: _buildActionKey('확인', Colors.green, _checkGuess, screenWidth),
                    ),
                    SizedBox(width: screenWidth * 0.02),
                    Expanded(
                      child: _buildActionKey('지우기', Colors.red, _clearInput, screenWidth),
                    ),
                  ],
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildKey(String value, double screenWidth) {
    return GestureDetector(
      onTap: () {
        if (_controller.text.length < 4) { // 입력 제한 4자리로 변경
          setState(() {
            _controller.text += value;
          });
        }
      },
      child: Container(
        margin: EdgeInsets.all(screenWidth * 0.01),
        height: screenWidth * 0.15,
        alignment: Alignment.center,
        decoration: BoxDecoration(
          color: Colors.blue,
          borderRadius: BorderRadius.circular(8),
        ),
        child: Text(
          value,
          style: TextStyle(fontSize: screenWidth * 0.05, color: Colors.white),
        ),
      ),
    );
  }

  Widget _buildActionKey(String value, Color color, Function() onTap, double screenWidth) {
    return GestureDetector(
        onTap: onTap,
        child: Container(
        margin: EdgeInsets.all(screenWidth * 0.01),
    height: screenWidth * 0.15,
    alignment: Alignment.center,
    decoration: BoxDecoration(
    color: color,
    borderRadius: BorderRadius.circular(8),
    ),
    child: Text(
    value,
    style: TextStyle(
    fontSize: screenWidth * 0.05,
    color: Colors.white,
    ),
    ),
    ),
    );
  }
}

</pre>
</code>

결과 화면
![image](https://github.com/user-attachments/assets/2d0e36b5-a0ab-4b31-be9e-d416389afd99)
![image](https://github.com/user-attachments/assets/fafc07fa-67f6-4de3-a5ee-cc1e1b8c6715)




