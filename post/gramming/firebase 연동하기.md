# Flutter Fidrebase

## # Firebase 프로젝트 생성

[link](https://firebase.google.com)

- 로그인`  >  `콘솔로 이동`  >  `프로젝트 만들기`  >  `프로젝트 이름 입력  
` >  `Google 애널리틱스 사용 여부 선택 및 설정`  >  `프로젝트 생성

<br>
 
## # iOS 설정
1. Firebase개요 - '+앱추가' - ios
2. Xcode - General - Bundle Identifier 변경하고 복사해놓기
3. 프로젝트 선택 - iOS버튼 클릭
4. 복사해놓은 Bundle Identifier 입력 - Register app 
5. `GoogleService-info.plist` 파일 다운로드
6. 위 파일을 ios / Runner 폴더에 넣기
7. firestore ios 설정 next 버튼
8. 적혀있는 데로 설정하고 완료

<br>
 
## # Android 설정
1. Gradle 수정
    ```dart
        // android/build.gradle
        buildscript {
            repositories {
                
                google()  
                mavenCentral()  

            }
            dependencies {
                ...
                classpath 'com.google.gms:google-services:4.3.15'
            }
        }

        allprojects {
            ...
            repositories {
                google() 
                mavenCentral()
            }
        }
    ```
    ```dart
    // android/app/build.gradle

        apply plugin: 'com.google.gms.google-services' // add

        applicationId "com.coqoa.gramming" // add

        dependencies {
            implementation platform('com.google.firebase:firebase-bom:31.5.0') // add
            implementation 'com.android.support:multidex:2.0.1' // add
        }
    ```
2. Firebase개요 - '+앱추가' - Android
3. 패키지 이름 입력(위에서 추가한 applicationId) - 앱 등록
4. google-services.json파일 다운로드, android / app파일 내부에 넣기

## # firebase_core 설치
[link](https://pub.dev/packages/firebase_core)

- 설치  
  `flutter pub add firebase_core`

<br>
 
- Podfile 수정
    ```dart
    // ios/Podfile
    platform :ios, '11.0'
    ```
    `cd ios`  
    `pod install`

<br>
 
- Firebase 초기화
    ```dart
    // main.dart
    import 'package:firebase_core/firebase_core.dart';

    Future<void> main() async {
        WidgetsFlutterBinding.ensureInitialized();
        await Firebase.initializeApp();

        runApp(MyApp());
    }
    ```

