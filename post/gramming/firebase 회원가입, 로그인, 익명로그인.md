# firebase 로그인, 회원가입, 익명로그인 기능 구현하기
- 유저의 이메일 주소와 비밀번호를 사용하여 인증구현

    > firebase와 프로젝트가 연동되어 있다는 가정하에 작성한 포스트입니다  
    > firebase 연동하기 포스트는 따로 작성해두었습니다.

## Firebase Authentication

1. firebase Authentication 탭에서 '이메일/비밀번호'를 활성화
2. firebase_auth: ^3.0.2 설치
3. 회원가입 페이지 구현
    ```dart
    final FirebaseAuth _auth = FirebaseAuth.instance;

    Future<UserCredential> signUpWithEmail(String email, String password) async {
    try {
        UserCredential userCredential =
            await _auth.createUserWithEmailAndPassword(
        email: email,
        password: password,
        );
        return userCredential;
    } on FirebaseAuthException catch (e) {
        if (e.code == 'weak-password') {
        print('The password provided is too weak.');
        } else if (e.code == 'email-already-in-use') {
        print('The account already exists for that email.');
        }
        return null;
    } catch (e) {
        print(e);
        return null;
    }
    }

    ```
    ```dart
    // validation

    Future<UserCredential> signUpWithEmail(String email, String password) async {
    
    }

    ```
   
4. 로그인 페이지 구현
    ```dart 
    import 'package:firebase_auth/firebase_auth.dart';

    final FirebaseAuth _auth = FirebaseAuth.instance;

    Future<UserCredential> signInWithEmail(String email, String password) async {
    try {
        UserCredential userCredential = await _auth.signInWithEmailAndPassword(
        email: email,
        password: password,
        );
        return userCredential;
    } on FirebaseAuthException catch (e) {
        if (e.code == 'user-not-found') {
        print('No user found for that email.');
        } else if (e.code == 'wrong-password') {
        print('Wrong password provided for that user.');
        }
        return null;
    }
    }

    ```
    ```dart
    // validation

    Future<UserCredential> signUpWithEmail(String email, String password) async {
    
    }

    ```
5. 익명로그인 구현
    ```dart 
    import 'package:firebase_auth/firebase_auth.dart';

    final FirebaseAuth _auth = FirebaseAuth.instance;

    Future<void> signInAnonymously() async {
        try {
            UserCredential userCredential = await _auth.signInAnonymously();
            User user = userCredential.user;
            // 익명 사용자 로그인 성공 시 처리할 코드
        } catch (e) {
            // 로그인 실패 시 처리할 코드
        }
    }

    ```
    5-1. 익명 로그인 -> 회원가입 구현
    ```dart 
    
    ```