# Flutter Fidrebase

## # Firebase 프로젝트 생성

[link](https://firebase.google.com)

1. 로그인
2. 콘솔로 이동
3. 프로젝트 만들기
4. 프로젝트 이름 입력
5. Google 애널리틱스 사용 여부 선택 및 설정
6. Create project

## # iOS 설정
1. Xcode - General - Bundle Identifier 변경
2. 프로젝트 선택 - iOS버튼 클릭
3. 변경한 Bundle Identifier 입력 - Register app 
4. `GoogleService-info.plist` 파일 다운로드
5. 위 파일을 ios / Runner 폴더에 넣기
6. firestore ios 설정 next 버튼
7. 적혀있는 데로 설정하고 완료

## # Android 설정
1. Firebase개요 - '+앱추가' - Android 클릭
2. 패키지 이름 입력 - 앱 등록
3. google-services.json파일 다운로드 - android / app파일 내부에 넣기
4. Firebase SDK 추가
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
## # firebase_core 설치
[link](https://pub.dev/packages/firebase_core)

- 설치  
  `flutter pub add firebase_core`
- Podfile 수정
    ```dart
    // ios/Podfile
    platform :ios, '11.0'
    ```
    `cd ios`  
    `pod install`
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

